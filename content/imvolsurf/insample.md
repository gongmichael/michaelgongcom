# 1.Estimation results
Below is the estimated parameters of [model (15)](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/Model.md). 
For identification, delta_l is fixed to 1. Transition matrix and state covariance matrix are set to block diagonal. 

```python
(1) Parameters for scaling equity loadings
=====  ========  ========  =======  =======  ========  ========
..       alpha0    alpha1    beta0    beta1    gamma0    gamma1
=====  ========  ========  =======  =======  ========  ========
AAPL      0.236     1.556    0.008    0.811    -0.008     0.709
CVX       0.057     1.180    0.001    0.862    -0.006     0.822
DIS       0.050     0.955   -0.003    0.978    -0.003     0.774
CMCSA     0.017     0.942   -0.000    1.112     0.001     0.527
MO        0.108     1.125    0.001    0.670     0.014     0.756
LLY       0.095     1.127   -0.000    0.770     0.012     0.737
CL        0.066     0.538   -0.000    0.476     0.002     0.560
DOW       0.029     1.304   -0.006    0.992    -0.003     0.944
FDX       0.063     1.181   -0.006    0.830    -0.001     0.925
NSC       0.084     1.147   -0.002    0.912    -0.007     0.981
=====  ========  ========  =======  =======  ========  ========

(2) Parameters for collapsing idiosyncratic factors 
=====  =========  =========  =========
..       delta_l    delta_c    delta_s
=====  =========  =========  =========
AAPL           1     -0.058     -0.020
CVX            1     -0.090     -0.073
DIS            1     -0.033      0.040
CMCSA          1     -0.014      0.143
MO             1     -0.070      0.090
LLY            1     -0.141      0.148
CL             1     -0.007      0.023
DOW            1     -0.084      0.026
FDX            1     -0.071     -0.014
NSC            1     -0.054     -0.018
=====  =========  =========  =========

(3) Summary for state equation parameters

====  ====   =====   ============================   ============================== 
              vD                  mT                              mQ (x1E3) 
====  ====   =====   =====  ======  ======  =====   =====  ======  ======  ======  
mean  Idio  -0.000   0.991   0.000   0.000  0.000   0.044   0.000   0.000   0.000 
      level  0.000   0.000   0.994   0.061  0.018   0.000   0.090  -0.022   0.011 
      term   0.000   0.000  -0.002   0.958  0.005   0.000  -0.022   0.006  -0.002 
      smile  0.000   0.000   0.000  -0.006  0.990   0.000   0.011  -0.002   0.002 
                                                                                 
std   Idio   0.001   0.003   0.000   0.000  0.000   0.026   0.000   0.000   0.000 
      level  0.000   0.000   0.002   0.006  0.010   0.000   0.004   0.001   0.001 
      term   0.000   0.000   0.000   0.002  0.003   0.000   0.001   0.000   0.000 
      smile  0.000   0.000   0.000   0.002  0.001   0.000   0.001   0.000   0.000 
====  ====   =====   =====  ======  ======  =====   =====  ======  ======  ======   
```
Detail parameters click [here](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/Par.md).

# 2.Latent factors
The figure below plots the latent factors of economic restricted DFMX model.
* By design, level, term and smile capture the index latent factors.
* Idiosyncratic factor reminics the residual of level factor (click [here](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/preliminary.md) 
to review preliminary analysis on residual of [equation (5)-(7)](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/Model.md))
* Separate plot for each stock click [here](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/dfmxFactors_sep.png).
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/dfmxFactors.png)

# 3.In sample fit
The figure below plots the in sample RMSE of DFM and DFMX model aggregate at stock level.
* DFMX has slightly higher RMSE than DFM does. It might come from the collapse of idiosyncratic factors.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/RMSEIS.png)
* RMSE of entire surface click [here](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/HMIS.png).

# 4.Correlation
The figure below plots the correlation matrix of DFMX latent factors to investigate whether the idiosyncratic factor picks up 
the same information as index factros do.
* In general, the correlations between idiosyncratic factor and index factors are not high.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/corr.png)


# 5.R square
To investigate how important of each factor, I run the following regression

![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161121/regression.png)

where IV_{i,\tau,m,t} is the implied volatility of stock `i` in group `\tau,m` at
time `t`. `f_{i,j,t}` is the `j`th factor of stock `i` at time `t`. In our case,
`j` refers to `idio`,`level`,`term`,and `smile`, and `\tau,m` refers to `2, DOTM Put`
etc. 

The following figure plots the average r-square over option groups of each stocks. R square of entire surface click 
[here](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/rsquaredsurf.png).

![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/rsquared.png)

# 5.Variance decomposition
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/vardecompDoc.png)
* The following table reports the percentage of variance explained by index factors for each stock.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/vardecomp.png)


