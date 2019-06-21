# 1.Data Description
I choose the second largest stocks in each quintile of S&P 100 index. The moneyness 
and maturity grouping is organized as follow:
* 7-60 days: short term maturity.
* 60-120 days: medium term maturity.
* 120-240 days: long term maturity.

The moneyness grouping is the same as before. Below is the descriptive plotting of data. The term is defined as the difference of implied volatility between longest maturity and shortest maturity options. The smile structure is defined as the difference of implied volatility between DOTM Put and DOTM Call. The top 3 sub-plots average time series over stocks, and the bottom 3 sub-plots average time series over option groups. 

![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTMGlqTVJlQXE5WGM)


# 2. Economic restricted DFM latent factors (Time varying loading)
More detail investigation will be presented in [In-sample evaluation](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20170110/ISevaluate.md).
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTckMtTHBBMHpyUHc)

# 3. Relationship between equity latent factors and index latent factors 
The following table shows the regression results of equation (5)-(7) (see [model documentation](
    https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20161206/Model.md)).
```python
====  =========  ========  =========  ========  =========  ========  =====  =====  =====
         alpha0    alpha1      beta0     beta1     gamma0    gamma1  1:r2   2:r2   3:r2
====  =========  ========  =========  ========  =========  ========  =====  =====  =====
XOM    0.056***  0.886***  -0.001***  0.857***  -0.004***  0.998***  0.753  0.579  0.734
INTC   0.091***  1.194***  -0.005***  1.027***   0.001     0.805***  0.638  0.662  0.378
UNH    0.058***  1.272***  -0.003***  1.023***   0.003***  0.877***  0.742  0.449  0.437
DD     0.027***  1.188***  -0.001***  1.011***  -0.001*    0.914***  0.944  0.729  0.622
COF   -0.082***  2.486***  -0.011***  2.027***   0.007***  1.247***  0.817  0.584  0.307
====  =========  ========  =========  ========  =========  ========  =====  =====  =====
```
The following figure plots the residual from regression (5)-(7).
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTX3R0X2RHOTBUc1k)

The table below reports the variance explained by first 3 principal components. (by decomposing of variance and decomposing
of correlation matrix, repectively).
```python 
==============================================
           Covariance          Correlation 
      ==================   ===================
        1st    2nd   3rd     1st    2nd    3rd
====  =====  =====  ====   =====  =====  =====
XOM   94.03   4.10  1.87   53.06  31.26  15.67
INTC  96.95   2.53  0.51   64.47  27.49   8.04
UNH   93.66   5.09  1.24   60.42  30.91   8.67
DD    80.94  16.08  2.96   49.64  37.60  12.76
COF   93.86   4.38  1.76   68.73  19.59  11.68
==============================================
```
