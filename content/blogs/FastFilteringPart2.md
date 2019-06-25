---
title: "Fast Kalman Filtering with univariate treatment Part II---Fast C++ implementation with PyBind11"
date: 2018-06-03T12:52:36+06:00
image: images/blog/blog-post-1.jpg
author: Michael Gong
featured: true
thumbnail: "/img/posts/FastFiltering1/kalman.png"
tags: ["Kalman Filter","Univariate","Python", "C++", "PyBind11"]
---

# Fast Kalman Filtering with univariate treatment Part II---Fast C++ implementation with PyBind11

If you haven't read the [Part I](https://michael-gong.com/blogs/fastfilteringpart1/) of the this series, please read [it now](https://michael-gong.com/blogs/fastfilteringpart1/). In the Part [Part I](https://michael-gong.com/blogs/fastfilteringpart1/), we see that the univariate filter can successfully extract the signal from the noisy measurement. However, in pure Python/Numpy environment, looping over all time steps and all measurements is very costly. The timing experiment shows that the univariate filter actually runs slower than the conventional Kalman Filter. In this article, I will show the implementation in C++ with Eigen and PyBind11.

***prerequisites of this tutorial***

- GCC >= 7.0.0
- [PyBind11](https://github.com/pybind/pybind11)
- [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page)
- [CMake](https://cmake.org/)

## 1. Implementation in Eigen (c++)

The code below is the implementation of the univariate filter in C++. Note that

- I set the state dimension as 4 as illustration. For more general usage, you should change the state dimension accordingly. The real advantage of using fixed size array of Eigen is the stunning performance when the dimension is known during compilation. This allows Eigen to do more aggressive optimization. 
- Unlike Numpy, Eigen does not (naively) support 3-dimensional array (though you can use tensor in Eigen's unsupported feature). So, it is pretty challenging when the dimension of the measurement is time varying. To solve this, we need to transform the unbalance data to "pseudo" balance data. That is, we can allocate a two-dimensional array whose size is *"max length of the measurement"* $\times$ *"time steps"*. Then pad measurements whose length is less than the max length with 0. Inside the filtering function, we also provide a (integer) vector $vS$ to tell the algorithm what's the dimension of the measurements at each time step.
- Another performance bottleneck is the measurement loading matrix $mZ$. Numpy use *row-major* layout, but Eigen use "column-major" layout. Since univariate filter will loop over each row of the measurement loading $Z$, accessing each row of the $Z$ is costly since the memory is not continuous. One way to deal with memory layout problem is to transpose the loading matrix $Z$ and copy as *column-major*. So, instead of looping over each row, we loop over each column of the transposed $Z$.
- Last, similar to padding of measurements, we also need to pad the loading matrix $Z$ and disturbance covariance $H$. 

`uniFilter.cpp`
```c++
#include <cmath>
#include <iostream>
#define EIGEN_USE_MKL_ALL
#define EIGEN_INITIALIZE_MATRICES_BY_ZERO
#include <pybind11/eigen.h>
#include <pybind11/numpy.h>
#include <pybind11/pybind11.h>
#include <pybind11/stl.h>
#include <eigen3/Eigen/Dense>
#include <tuple>

using namespace Eigen;
using namespace std;
namespace py = pybind11;

typedef Eigen::Matrix<double, 4, -1> Matrixfd;
std::tuple<Matrixfd, Matrixfd, Matrixfd, Matrixfd>
univariateFilter(Eigen::Ref<Vector4d> a1, Eigen::Ref<Matrix4d> p1,
                 Eigen::Ref<Matrixfd> mZf,Eigen::Ref<MatrixXd> mH,
                 Eigen::Ref<Vector4d> vD, Eigen::Ref<Matrix4d> mT,
                 Eigen::Ref<Matrix4d> mQ, Eigen::Ref<MatrixXd> mY,
                 Eigen::Ref<VectorXi> vS) {
    int N = mY.cols(), maxM = mY.rows(), m = 0;

    //Allocate memory to store the results
    Matrixfd aFf(4, N);
    Matrixfd aPf(4, (N + 1));
    Matrixfd aSf(4, N);
    Matrixfd pFf(4, N * 4);
    MatrixXd v(maxM, N), F(maxM, N), mKf(4, maxM * N);

    aPf.col(0) = a1;
    pPf.leftCols(4) = p1;

    Vector4d a = a1;
    Matrix4d P = p1;

    // Construct the Map object, so we don't need to copy the matrix during iteration
    Map<Vector4d> aF(NULL);
    Map<Matrix4d> pF(NULL);
    Map<Vector4d> aP(NULL);
    Map<Matrix4d> pP(NULL);

    Map<Matrixfd> mK(NULL, 4, maxM);
    Map<Matrixfd> mZ(NULL, 4, maxM);
    Map<Vector4d> mZp(NULL);
    Map<Vector4d> mKp(NULL);
    
    for (int t = 0; t < N; t++) { // Time step filtering
        
        // Accessing the results columns or blocks 
        new (&aF) Map<Vector4d>(aFf.col(t).data());
        new (&pF) Map<Matrix4d>(pFf.col(t * 4).data());
        new (&aP) Map<Vector4d>(aPf.col(t + 1).data());
        new (&pP) Map<Matrix4d>(pPf.col((t + 1) * 4).data());

        // Retrieving the loading matrix block and Kalman Gain block
        new (&mK) Map<Matrixfd>(mKf.col(maxM * t).data(), 4, maxM);
        new (&mZ) Map<Matrixfd>(mZf.col(maxM * t).data(), 4, maxM);

        m = vS(t);
        for (int i = 0; i < m; i++) {
            // Retrieving the loading matrix columns and Kalman Gain columns
            new (&mZp) Map<Vector4d>(mZ.col(i).data());
            new (&mKp) Map<Vector4d>(mK.col(i).data());

            v(i, t) = mY(i, t) - mZp.adjoint() * a; // error decomposition
            F(i, t) = mZp.adjoint() * P * mZp + mH(i, t); // measurement variance
            mKp = P * mZp / F(i, t); // Kalman gain

            if (F(i, t) != 0) {
                a += mKp * v(i, t); // filtering state
                P -= F(i, t) * mKp * mKp.transpose(); // filtering state variance
            }
        }
        aF = a;
        pF = P;
        aP = mT * a + vD; // predicting state
        pP = mT * P * mT.transpose() + mQ; // predicting state variance
    }


    return std::make_tuple(aFf, aPf, aSf, pFf);
};

// initialize the PyBind11 module
void init_univariateFilter(py::module& m) {
    m.def("univariateSmooth", &univariateSmooth);
}
```

Once we finish the implementation of the univariate filter, we need to wrap it into PyBind11 module.

`Index.cpp`
```c++
#include <pybind11/pybind11.h>

namespace py = pybind11;
void init_univariateFilter(py::module&);

PYBIND11_MODULE(kfcore, m) {
    m.doc() = "pybind11 Univariate Ganssian Kalman Filter";
    init_univariateFilter(m);
}
```

I use CMake to compile the module. For more information of the CMake compilation for the PyBind11, [click here](https://pybind11.readthedocs.io/en/master/compiling.html#building-with-cmake).

`CMakeLists.txt`
```cmake
cmake_minimum_required(VERSION 3.9)
project(kfcore)
# Set C++ 17 Standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(mkl_DIR ${CMAKE_CURRENT_LIST_DIR})
set(CMAKE_CXX_FLAGS "-Wall -O3 -mavx2 -fopenmp")

include_directories("/home/michael/anaconda3/include/python3.6m")
find_package(pybind11 REQUIRED)

file(GLOB_RECURSE SOURCES RELATIVE ${CMAKE_SOURCE_DIR} "*.cpp")
pybind11_add_module(kfcore ${SOURCES})
target_link_libraries(kfcore PUBLIC mkl_rt mkl_core mkl_intel_thread mkl_intel_lp64 iomp5 pthread dl m)
install(
  TARGETS 
  kfcore 
  DESTINATION ${PROJECT_SOURCE_DIR})
```

Finally, we can build the module by

```shell
mkdir build
cd build/
cmake ..
make install
```

# 2. Test the performance

**Let's generate some data** similar to the [Part I](https://michael-gong.com/blogs/fastfilteringpart1/) of this series


```python
def genStates(vD, mT, mQ, N):
    eta = np.random.multivariate_normal(vD, mQ, [N])
    
    f = np.zeros_like(eta)
    for i in range(1,N-1):
        f[i+1] = vD + mT@f[i] + eta[i]
    return f

def genMeasure(mZ, mH, f):
    N = f.shape[0]
    M = mZ.shape[0]
    y = np.zeros([M, N])
    eps = np.random.multivariate_normal(np.zeros(M), mH, [N])
    y = np.zeros_like(eps).T
    for i in range(1, N-1):
        y[:,i] = mZ@f[i] + eps[i]
    return y

np.random.seed(123)
N = 200
r = 4
m = 10
mZ = np.random.rand(m,r)
mH = np.diag(np.random.normal(0,1,m)**2)

vD = np.zeros(4)
mT = np.eye(r) * 0.97
mQ = np.random.normal(0,1,[r,r])
mQ = mQ.T@mQ

f = genStates(vD,mT,mQ,N)
y = genMeasure(mZ,mH,f)
```

**Preparing the data dimension requried by our C++ univariate filter (tranposing and padding)**


```python
import kfcore


```python
a1 = np.zeros(r,order='F')
p1 = np.eye(r,order='F')
maxM = m # In this case, the dimension of measurement is constant
Z = np.zeros([r, N*maxM], order='F')
H = np.zeros([maxM, N], order='F')
Y = np.zeros([maxM ,N], order='F')
for i in range(N):
    Z[:, i*maxM:(i+1)*maxM] = mZ.T # Transpose
    Y[:,i] = y[:,i]
    H[:,i] = np.diagonal(mH)
    
vD, mT, mQ = map(np.asfortranarray,[vD,mT,mQ])
vS = np.ones(N,dtype=np.int32)*r

vC = np.zeros([m, N],order='F')
mG = np.zeros([r,r], order='F')
mU = np.ones([r,N],order='F')
```

**Run the C++ univariate filter**


```python
aFf, aPf, aSf, pFf = kfcore.univariateFilter(a1,p1,Z,H,vD,mT,mQ,Y,vS)
```

**Compare the results with Python/Numpy univaraite filter** implemented in [Part I](https://michael-gong.com/blogs/fastfilteringpart1/)


```python
Z = np.zeros([N,m,r])
H = np.zeros([N,m,m])
Y = np.zeros([N,m,1])
for i in range(N):
    Z[i], H[i],Y[i] = mZ,mH,y[:,[i]]
sizes = np.ones(N,dtype=np.int32)*r

# Filtering
aF, pF, aP, pP = UniFilter(a1, p1, Z, H, vD, mT, mQ, Y, sizes)

print("Is the output of C++ implmentation same as Python implmentation?", np.allclose(aF, aFf))
```

    Is the output of C++ implmentation same as Python implmentation? True


**Let's time the performance**


```python
%timeit kfcore.univariateFilter(a1, p1, Z, H, vD, mT, mQ, Y, vS)
```

    80.8 µs ± 1.1 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)



```python
%timeit UniFilter(a1, p1, Z, H, vD, mT, mQ, Y, sizes)
```

    18.9 ms ± 881 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)



```python
print(f"C++/Python speed up: {round(18900/80.8, 2)}x")
```

    C++/Python speed up: 233.91x


**We achieve 234 times speed up!**
