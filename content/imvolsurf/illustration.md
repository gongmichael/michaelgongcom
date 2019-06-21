# Illustration using one stock (Johnson & Johnson:JNJ)

**Overview of last two meetings**
* Meeting with @Dick
 + Issues: Identification of both constants in measurement equation and state equation.
 + Solved: Removing the state constants is enough to identify DFMX.
 + Remain: Strange identification (let row 6,9,10,12 unchanged) strategy yields higher likelihood. 

* Meeting with @Michel
 + Issues: Strange identification with higher likelihood: too restrictive loading of economic DFMX?
 + Solved: Implementing a fully efficient DFMX with/without restricted measurement constants 
 + Remain: ?

In this update, I implemented the following two variants of DFMX:

**Version 1:**
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTVm54YVRxb1pFdE0)
**Version 2:**
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTbXNJaEw4Uk11dEU)

Below is the summary of the number of parameters of DFM and DFMX:
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTNzFnX0hkUndYc2c)
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTRGdXLUwzcm5OMGM)

In the rest of reports, the following abbreviations stands for:
* `ecoDfm`: Economic restricted DFM
* `fulDfm`: Fully efficient DFM 
* `ecoDfmX1`: Econoimc restricted DFMX version 1
* `ecoDfmX2`: Econoimc restricted DFMX version 2
* `fulDfmX1`: Fully efficient DFMX version 1
* `fulDfmX2`: Fully efficient DFMX version 2   

Before I reports the results, below is some remarks of this update:
* Estimation is not a problem anymore. I choosed a better BFGS library
and implemented the univariate treatment. Now the estimation time of fully 
efficient DFM is down to 5~10 seconds, and that of fully efficient DFMX version 2
is about 30~40 seconds. Of course, I tripple checked the new BFGS library. 
With the new library, the likelihood uses ~1/20 amount of time to climb to point 
where previous library stops. And the likelihood provided by the new library is 
never lower than the one provide by old library.

* Because performance of estimation dramatically increase, every DFMX model in this 
update is in **one-step** manner. The stopping criteria is set to "extreme". That is,
the decrease of likelihood is less than the machine precision(10E-16) **and** the norm
of gradient is less than 10E-2.

* The standard error of parameters might be not robust. Since the inverse hessian is 
approximiated by BFGS algorithm, which is based the calculation of gradient vector.
However, our calculation of gradient vertor uses central difference, with naive step 
size of 10E-8, which might result in inaccurate gradient.


# 1. Summary of DFM and DFMX 
The table below shows the summary statistics of estimation:
```python
__________________________________________________________________________________
                ecoDfm      fulDfm    ecoDfmX1    ecoDfmX2    fulDfmX1    fulDfmX2
__________________________________________________________________________________
No. pars            36          78          75          90         159         174    
aic        -350936.232 -373342.656 -744876.158 -778835.226 -812162.803 -813701.047
bic        -350708.954 -372857.387 -744409.317 -778276.246 -811179.980 -812626.084
loglike     175505.116  186750.328  372514.079  389508.613  406241.401  407025.524
LR-stats                 22490.424               33989.069   33465.576    1568.245
alpha1                                0.590***    0.594***    0.569***    0.585***
beta1                                 0.589***    0.589***    0.372***    0.423***
gamma1                                0.263***    0.282***    0.372***    0.375***
F(p-value)                            0.000       0.000       0.000       0.000
__________________________________________________________________________________
```
* `No.pars`: Number of parameters
* `aic`: Akaike information criterion
* `bic`: Bayesian information criterion 
* `loglike`: log likelihood 
* `LR-stats`: Likelihood ratio test statistics testing against one left model.
* `alpha1`: The level scaling factor 
* `beta1`: The term scaling factor 
* `gamma1`: The smile scaling factor 
* `***`: coefficient significant at 1% level. (The parameter covariance matrix
is approximiated by the inverse Hessian provided by BFGS algorithm)
* `F(p-value)`: The p-value of F statistics testing the null hypothesis that 
`alpha1`, `beta1`, and `gamma1` are *jointly* zeros.

# 2. DFM smoothed factors
The figure below plots the smoothed latent factors of `ecoDfm` and `fulDfm`.
The `Level` and `Term` factors are similar, and the `Smile` factor of these
models are different to some extent. The reason might be the strong restriction
imposed by `ecoDfm` (as we can see in the following loading matrix plot).
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTM2JSYlZFalRnTjg)
# 3. DFMX smoothed factors 
The figure below plots the smoothed latent factors of DFMX models. Except for 
`Smile` factor, we cannot tell difference between `ecoDfmX` and `fulDfmX`.
Besides, visually speaking, version 1 and version 2 are quite similar for 
both economic DFMX and fully efficient DFMX.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTWkllcTVoenRNaTg)
# 4. Loading matrix 
The figure below plots the loading matrix of DFMX models. Here we can see 
that the smile loading of fully efficient DFMX dramatically different from
that of economic restricted DFMX. This is why we observe different `Smile` 
factor above. Although the `Level` loading are also quite different, but the 
scale is muc smaller compared with the `Smile` loading.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTNDk0bTdva19PNXM)

Full parameters can be viewed here:
* [ecoDfm](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20170222/Figs/ecoDfm.md)
* [fulDfm](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20170222/Figs/fulDfm.md)
* [ecoDfmX1](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20170222/Figs/ecoDfmX1.md)
* [ecoDfmX2](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20170222/Figs/ecoDfmX2.md)
* [fulDfmX1](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20170222/Figs/fulDfmX1.md)
* [fulDfmX2](https://github.com/gongmichael/ImvolSurf/blob/master/imvolsurf/meeting/20170222/Figs/fulDfmX2.md)


# 5. Variance Decmposition
The figure belows shows the variance decomposition, which reports the 
percentage of the variance of idiosyncratic factors explained by index factors.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTY2JNZWM2bmNybWc)
# 6. Fit
Finally, the two figure below show the in-sample fit in terms of RMSE. The 
first figure shows RMSE at aggregate level (pooling multivariate time series), 
and the second figure shows the RMSE over implied volatility surface. Clearly,
fully efficient DFMX version 2 is winner here (not surprise due to its number 
of parameters).
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTOFJmbEpkckw1ZzQ)
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTdjVQX0N0bXV1NVk)




