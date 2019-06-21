# 1.Estimation results
Below is the estimated parameters of [model (15)](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/Model.md). 
For identification, delta_l is fixed to 1. Transition matrix and state covariance matrix are set to block diagonal. 

```python
(1) Parameters for scaling equity loadings
=====  ========  ========  =======  =======  ========  ========
..       alpha0    alpha1    beta0    beta1    gamma0    gamma1
=====  ========  ========  =======  =======  ========  ========
AAPL      0.236     0.770    0.001    0.458    -0.008     0.422
CVX       0.068     0.958    0.013    0.847     0.003     0.588
DIS       0.05      0.862    0.002    0.722     0.010     0.480
CMCSA     0.016     0.874    0.005    0.688    -0.000     0.366
MO        0.106     0.585    0.007    0.400     0.009     0.429
LLY       0.087     0.632   -0.010    0.480     0.013     0.289
CL        0.072     0.510    0.007    0.409     0.005     0.232
DOW       0.028     1.015   -0.003    0.812     0.004     0.252
FDX       0.075     0.802    0.001    0.658     0.005     0.425
NSC       0.082     0.775   -0.002    0.687    -0.003     0.288
=====  ========  ========  =======  =======  ========  ========


(2) Summary for idiosyncratic state equation parameters

====  ====   =====   =====================   ======================
              vD           mT                       mQ (x1E3) 
====  ====   =====   ======  =====  ======   ======  ======  ======  
mean  level  0.001    0.992  0.035  -0.004    0.051  -0.011   0.003 
      term  -0.000   -0.001  0.953   0.004   -0.011   0.005  -0.001 
      smile  0.000    0.002  0.004   0.986    0.003  -0.001   0.003 
                                                                   
std   level  0.000    0.004  0.037   0.021    0.024   0.006   0.004 
      term   0.000    0.001  0.009   0.004    0.006   0.003   0.001 
      smile  0.000    0.001  0.005   0.005    0.004   0.001   0.002 
====  ====   =====   ======  =====  ======   ======  ======  ====== 
```
Detail parameters click [here](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/Par3i.md).

# 2.Latent factors
The figure below plots the latent factors of economic restricted DFMX model.
* Separate plot for each stock click [here](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/dfmxFactors_sep3i.png).
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/dfmxFactors3i.png)

# 3.In sample fit
The figure below plots the in sample RMSE of DFM and DFMX model aggregate at stock level.
* DFMX has slightly higher RMSE than DFM does. It might come from the collapse of idiosyncratic factors.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/RMSEIS3i.png)
* RMSE of entire surface click [here](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/HMIS3i.png).

# 4.Correlation
The figure below plots the correlation matrix of DFMX latent factors to investigate whether the idiosyncratic factor picks up 
the same information as index factros do.
* In general, the correlations between idiosyncratic factor and index factors are not high.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/corr3i.png)


# 5.R square
To investigate how important of each factor, I run the following regression

![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/regression.png)

where IV_{i,\tau,m,t} is the implied volatility of stock `i` in group `\tau,m` at
time `t`. `f_{i,j,t}` is the `j`th factor of stock `i` at time `t`. In our case,
`j` refers to `idio`,`level`,`term`,and `smile`, and `\tau,m` refers to `2, DOTM Put`
etc. 

The following figure plots the average r-square over option groups of each stocks. R square of entire surface click 
[here](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/rsquaredsurf3i.png).

![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/rsquared3i.png)

# 5.Variance decomposition
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/vardecompDoc.png)
* The following table reports the percentage of variance explained by index factors for each stock.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/vardecomp3i.png)


