## (2) Re-estimated DFMX model (equation 9 - 12)


### (2.1) plot of factors 
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/dfmxFactors_revisit.png)
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/dfmxFactors_revisit_sep.png)

### (2.2) r square 
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/rsquared_revisit.png)
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/rsquaredsurf_revisit.png)

### (2.3) correlation 
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/corr_revisit.png)

### (2.4) parameters 
```python 
=====  ========  ========  =======  =======  ========  ========
..       alpha0    alpha1    beta0    beta1    gamma0    gamma1
=====  ========  ========  =======  =======  ========  ========
AAPL      0.23      1.689    0.001    0.625    -0.004     0.71
CVX       0.05      1.221   -0.002    0.768    -0.008     0.788
DIS       0.044     1.183   -0.002    0.938    -0.002     0.824
CMCSA     0.004     1.509   -0        1.046     0.001     1.004
MO        0.105     1.212    0.001    0.619     0.017     0.808
LLY       0.09      1.132   -0.003    0.673     0.014     0.722
CL        0.075    -7.856    0.003    0.448    -0.007     0.029
DOW       0.01      1.441   -0.005    1.059    -0.003     0.959
FDX       0.051     1.286   -0.001    0.871    -0.005     0.918
NSC       0.076     1.356   -0.003    0.914    -0.007     0.965
=====  ========  ========  =======  =======  ========  ========


=====  =========  =========  =========
..       lambda1    lambda2    lambda3
=====  =========  =========  =========
AAPL       1.148     -0.068     -0.021
CVX        0.856     -0.073     -0.066
DIS        0.744     -0.037      0.027
CMCSA      0.859     -0.004      0.162
MO         1.068     -0.083      0.101
LLY        0.994     -0.132      0.15
CL         5.187      0.001      0.06
DOW        0.835     -0.107      0.019
FDX        0.819     -0.083     -0.013
NSC        0.87      -0.066     -0.02
=====  =========  =========  =========


====  ====   =====   ============================   ============================== 
              vD                  mT                              mQ (x1E3) 
====  ====   =====   =====  ======  ======  ======   ======  ======  ======  ======  
mean  Idio   0       0.99    0       0       0        0.067  -0.016   0.003  -0.002  
      level  0.001   0       0.996   0.001  -0.015   -0.016   0.067  -0.017   0.011  
      term   0       0      -0.003   0.978   0.014    0.003  -0.017   0.005  -0.003  
      smile  0       0      -0      -0.007   0.99    -0.002   0.011  -0.003   0.002  
                                                                                   
std   Idio   0.001   0.003   0       0       0        0.042   0.044   0.011   0.007  
      level  0.001   0       0.003   0.021   0.01     0.044   0.002   0.001   0.001  
      term   0       0       0.001   0.005   0.002    0.011   0.001   0       0      
      smile  0       0       0.001   0.004   0.002    0.007   0.001   0       0      
====  ====   =====   =====  ======  ======  ======   ======  ======  ======  ======  
```
**Full parameters reports could be found** [here](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161121/Par_revisit.md).

### (2.5) Fit 
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/RMSEIS_revist.png)
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/HMIS_revisit.png)
