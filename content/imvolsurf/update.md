# 1. Fit errors of different trainning sample
In this update, I've tried 3 different grouping, and investigate their in-sample properties. 

* Original grouping, grouping sample by 7-80,80-160,160-240 maturity days.
* Unequal grouping, grouping sample by 7-30,30-60,60-120,120-240, as suggested by Dick.
* Even grouping, grouping sample by 7-60,60-120,120-180,180-240, as comparison for uneven grouping.

In order to investigate the fit of different grouping, I calculate the fitted values of `ALL`
options by economic restricted model, then report the fit metrics.

![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/RMSE.png)
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/MAE.png)
# 2. Linear regression 


```python 
Original grouping 
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======
         alpha0    alpha1      beta0     beta1     gamma0    gamma1   1:r2   2:r2    3:r2
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======
AAPL   0.232***  0.795***  -0.004***  0.531***  -0.01***   0.942***  0.341  0.182   0.520
CVX    0.058***  0.929***  -0.001***  0.739***  -0.007***  0.986***  0.780  0.623   0.807
DIS    0.05***   1.185***  -0.003***  0.961***  -0.002***  0.911***  0.841  0.700   0.649
CMCSA  0.014***  1.535***  -0.001***  1.026***  0.002***   1.0***    0.802  0.612   0.437
MO     0.109***  0.625***   0.001***  0.534***  0.015***   0.435***  0.398  0.276   0.166
LLY    0.094***  0.79***   -0.002***  0.575***  0.013***   0.404***  0.592  0.191   0.114
CL     0.066***  0.685***  -0.0       0.434***  0.001***   0.593***  0.825  0.454   0.502
DOW    0.03***   1.518***  -0.005***  0.915***  -0.002***  1.056***  0.778  0.526   0.608
FDX    0.063***  1.134***  -0.003***  0.698***  -0.004***  0.973***  0.878  0.572   0.664
NSC    0.083***  1.138***  -0.003***  0.865***  -0.005***  1.091***  0.766  0.626   0.733
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======

Equal grouping 
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======
         alpha0    alpha1      beta0     beta1     gamma0    gamma1   1:r2   2:r2    3:r2
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======
AAPL   0.233***  0.784***  -0.003***  0.561***  -0.01***   0.927***  0.326  0.170   0.532
CVX    0.057***  0.932***  -0.001***  0.769***  -0.007***  0.965***  0.772  0.600   0.809
DIS    0.048***  1.189***  -0.002***  0.995***  -0.001***  0.894***  0.836  0.685   0.652
CMCSA  0.011***  1.546***  -0.0       1.124***  0.002***   0.994***  0.802  0.612   0.450
MO     0.109***  0.625***  0.001***   0.592***  0.015***   0.439***  0.391  0.270   0.177
LLY    0.093***  0.795***  -0.002***  0.62***   0.013***   0.398***  0.591  0.191   0.114
CL     0.065***  0.688***  -0.0       0.434***  0.002***   0.576***  0.829  0.444   0.512
DOW    0.025***  1.542***  -0.005***  1.015***  -0.001**   1.03***   0.779  0.474   0.605
FDX    0.061***  1.144***  -0.003***  0.744***  -0.003***  0.958***  0.879  0.552   0.679
NSC    0.081***  1.145***  -0.003***  0.951***  -0.005***  1.062***  0.760  0.606   0.730
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======

Unequal grouping 
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======
         alpha0    alpha1      beta0     beta1     gamma0    gamma1   1:r2   2:r2    3:r2
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======
AAPL   0.232***  0.798***  -0.003***  0.658***  -0.008***  0.903***  0.342  0.154   0.517
CVX    0.061***  0.916***  -0.001***  0.743***  -0.007***  0.987***  0.775  0.604   0.805
DIS    0.053***  1.184***  -0.003***  1.007***  -0.001***  0.879***  0.841  0.681   0.648
CMCSA  0.017***  1.526***  -0.001***  1.114***  0.003***   0.943***  0.818  0.619   0.434
MO     0.108***  0.624***  0.001***   0.583***  0.014***   0.401***  0.400  0.270   0.162
LLY    0.1***    0.77***   -0.003***  0.577***  0.013***   0.379***  0.559  0.177   0.107
CL     0.07***   0.668***  -0.0***    0.449***  0.002***   0.56***   0.821  0.445   0.489
DOW    0.042***  1.477***  -0.006***  0.963***  -0.0       0.986***  0.764  0.478   0.578
FDX    0.069***  1.118***  -0.003***  0.765***  -0.002***  0.947***  0.868  0.560   0.663
NSC    0.088***  1.125***  -0.004***  0.916***  -0.003***  1.03***   0.763  0.588   0.703
=====  ========  ========  =========  ========  =========  ========  =====  =====  ======
```

# 3. PCA on Residuals
First 3 PCA explains (in %)
```python 
Original grouping 
=====  ==================      ====================
       Use Covariance           Use Correlation
=====  ==================      ====================
AAPL   96.82  1.87  1.31       57.53   29.35  13.11
CVX    95.41  3.05  1.54       54.94   31.48  13.58
DIS    93.69  4.34  1.97       55.85   30.56  13.59
CMCSA  94.85  3.41  1.73       58.42   33.01   8.57
MO     94.47  3.96  1.56       62.36   23.76  13.88
LLY    91.73  5.72  2.54       70.97   20.35   8.68
CL     87.65  9.99  2.36       50.84   36.72  12.45
DOW    95.38  3.57  1.05       54.5    35.55   9.96
FDX    90.73  7.18  2.09       58.75   29.52  11.73
NSC    95.25  3.47  1.29       53.4    33.7   12.9 
=====  =====  ====  ====       =====  ======  =====

Equal grouping
=====  ==================      ====================
       Use Covariance           Use Correlation
=====  ==================      ====================
AAPL   96.93   1.86  1.22      57.88   30.07  12.05
CVX    95.67   2.96  1.36      56.67   31.5   11.84
DIS    93.81   4.44  1.74      55.22   32.29  12.49
CMCSA  94.89   3.38  1.73      58.35   33.36   8.29
MO     94.47   3.9   1.63      62.37   24.28  13.35
LLY    91.72   5.77  2.51      71.47   19.75   8.77
CL     87.62  10.12  2.25      49.61   37.19  13.2 
DOW    94.99   3.67  1.35      54.33   35.34  10.33
FDX    90.75   7.06  2.18      57.28   31.29  11.43
NSC    95.17   3.49  1.33      53.17   34.43  12.4 
=====  =====  =====  ====      =====  ======  =====

Unequal grouping 
=====  ==================      ====================
       Use Covariance           Use Correlation
=====  ==================      ====================
AAPL   96.09  2.45  1.46       56.1    31.65  12.25
CVX    95.63  2.93  1.44       55.82   32.11  12.07
DIS    94.23  4.01  1.76       57.77   31.51  10.71
CMCSA  94.52  3.67  1.81       59.63   31.66   8.71
MO     95.01  3.27  1.73       62.98   24.24  12.78
LLY    93.41  4.51  2.08       73.53   19.35   7.13
CL     88.59  9.21  2.2        53.6    35.6   10.81
DOW    95.5   3.29  1.21       55.16   35.16   9.68
FDX    91.72  6.38  1.9        58.94   31.65   9.41
NSC    95.08  3.51  1.41       53.94   34.38  11.68
=====  =====  ====  ====       =====  ======  =====

```

# 4. Variance Decomposition
**DFMX1i Unequal grouping**
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/vardecompU.png)
[Original](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/vardecomp.png) grouping and [equal](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/vardecompE.png) grouping 


**DFMX3i Unequal grouping**
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/vardecomp3iU.png)
[Original](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/vardecomp3i.png) grouping and [equal](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161215/vardecomp3iE.png) grouping 







