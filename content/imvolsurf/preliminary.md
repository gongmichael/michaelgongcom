# 1. Descriptive plot 
Below is the descriptive plotting of data. The term is defined as the difference of implied volatility between
longest maturity and shortest maturity options. The smile structure is defined as the difference of implied 
volatility between DOTM Put and DOTM Call. The top 3 sub-plots average time series over stocks, and the 
bottom 3 sub-plots average time series over option groups.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/descriptive.png)

# 2. Economic restricted DFM latent factors 
The following figure plots the fully optimized economic restricted DFM for each stock.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/dfmRestrFactors.png)

# 3. Relationship between equity latent factors and index latent factors 
The following table shows the regression results of equation (5)-(7) (see [model documentation](
    https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/Model.md)).
```python
=====  ========  ========  =========  ========  =========  ========
         alpha0    alpha1      beta0     beta1     gamma0    gamma1
=====  ========  ========  =========  ========  =========  ========
AAPL   0.232***  0.795***  -0.004***  0.531***  -0.010***  0.942***
CVX    0.058***  0.929***  -0.001***  0.739***  -0.007***  0.986***
DIS    0.050***  1.185***  -0.003***  0.961***  -0.002***  0.911***
CMCSA  0.014***  1.535***  -0.001***  1.026***   0.002***  1.000***
MO     0.109***  0.625***   0.001***  0.534***   0.015***  0.435***
LLY    0.094***  0.79***   -0.002***  0.575***   0.013***  0.404***
CL     0.066***  0.685***  -0.000     0.434***   0.001***  0.593***
DOW    0.030***  1.518***  -0.005***  0.915***  -0.002***  1.056***
FDX    0.063***  1.134***  -0.003***  0.698***  -0.004***  0.973***
NSC    0.083***  1.138***  -0.003***  0.865***  -0.005***  1.091***
=====  ========  ========  =========  ========  =========  ========
```
The following figure plots the residual from regression (5)-(7), and the percentage of variance explained
by first principal is also depicted in each sub-plots.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/resid.png)

# 4. Investigation whether we can restrict mT to diagonal 
If we can restrict the transition matrix mT in state equation to diagonal, then we can further reduce the computation
burden. The table below reports the log-likelihood of (**economic restricted**) DFM and DFM with diagonal mT, respectively, 
and also shows likelihood ratio test and AIC results.
* LR test shows that we reject the digonal mT version of DFM 
* AIC confirms the rejection
* In the rest of works, I do not restrict the transition matrix of economic restricted DFM to diagonal. (**Caveats:** DFMX 
still has block diagonal structure of transition matrix).

```python 
=====  ======  ==========  ========  =======  ===========  =========  ==============
..        DFM    DFMdiagT      diff       LR      p-value    DFM_AIC    DFMdiagT_AIC
=====  ======  ==========  ========  =======  ===========  =========  ==============
AAPL   162420      162404  15.9359   31.8717  1.72677e-05    -324768         -324748
CVX    178149      178108  40.3204   80.6407  2.66454e-15    -356225         -356157
DIS    161679      161658  20.2688   40.5377  3.57086e-07    -323285         -323257
CMCSA  152295      152274  21.0861   42.1722  1.70026e-07    -304519         -304488
MO     150572      150563   8.5696   17.1391  0.008785420    -301072         -301067
LLY    163659      163635  23.2980   46.5959  2.25249e-08    -327245         -327211
CL     179288      179261  27.6979   55.3957  3.85631e-10    -358505         -358461
DOW    150453      150417  36.5697   73.1395  9.27036e-14    -300835         -300773
FDX    177178      177159  19.1824   38.3648  9.53158e-07    -354284         -354258
NSC    162507      162484  22.7190   45.4381  3.82982e-08    -324942         -324909
SPX    194588      194564  24.1380   48.2760  1.04070e-08    -389104         -389068
=====  ======  ==========  ========  =======  ===========  =========  ==============
```
