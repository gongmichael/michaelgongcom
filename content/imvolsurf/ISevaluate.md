# 1.DFM In-sample Analysis

## 1.1 Likelihood change 

```python
====  ======  =======  ======
..       dfm    dfmTV    diff
====  ======  =======  ======
XOM   170849   175721    4872
INTC  151422   155933    4510
UNH   155657   160427    4770
DD    167804   174027    6222
COF   134624   145016   10392
SPX   190523   195910    5386
====  ======  =======  ======
```

## 1.2 Parameters 

```python 
dfm 
====  ====   ======  ======================   ======================
              vD              mT                    mQ (x1E3) 
====  ====   ======  ======  ======  ======   ======  ======  ======  
mean  level  0.0004  0.9972  0.0747  0.0069   1.5372 -0.3412  0.0401 
      term   0.0005 -0.0019  0.9456 -0.0010  -0.3412  0.1101 -0.0174 
      smile -0.0002  0.0037 -0.0060  0.9712   0.0401 -0.0174  0.0739 
                                                               
std   level  0.0014  0.0070  0.0525  0.0205   0.4887  0.0985  0.0689 
      term   0.0006  0.0025  0.0186  0.0099   0.0985  0.0563  0.0241 
      smile  0.0004  0.0030  0.0088  0.0185   0.0689  0.0241  0.0639 
====  ====   ======  ======  ======  ======   ======  ======  ====== 

dfmTV
====  ====   ======  ======================   ======================
              vD              mT                    mQ (x1E3) 
====  ====   ======  ======  ======  ======   ======  ======  ======  
mean  level  0.0003  0.9982  0.085   0.0019   1.4859 -0.3278  0.0183 
      term   0.0006 -0.0024  0.9462  0.0031  -0.3278  0.1035 -0.0083 
      smile -0.0007  0.0049  0.0067  0.9745   0.0183 -0.0083  0.0861 
                                                               
std   level  0.0015  0.0074  0.0570  0.0205   0.4480  0.1187  0.0866 
      term   0.0006  0.0025  0.0179  0.0106   0.1187  0.0633  0.0106 
      smile  0.0012  0.0048  0.0299  0.0104   0.0866  0.0106  0.0816 
====  ====   ======  ======  ======  ======   ======  ======  ====== 
```

## 1.3 Factors 
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTRGd3UkZCSDJEWTQ)


# 2. DFMX (2 steps) In-sample Analysis
**Estimation Procedure**
* Estimate equity DFM and index DFM respectively.
* Use OLS to get parameters in equation (5)-(7).
* Obtain the residuals of OLS regression, compute the first 3
principal components. Regress the principal components on OLS 
residuals to get the loading matrix. Rotate the loading matrix to
economic restricted loading times scaling factors (alpha, beta, and
gamma).
* Use OLS on the rotated principal components to get the starting 
values of state dynamics.
* In order to identify the idiosyncratic factors and index factors,
the scaling factors are assumed to **not** affect the 6,9,10,and 11 
measurement equations.
* First, I estimated the DFMX model with constant loadings (taking 
average), and then use the parameters of optimized constant DFMX as 
starting valurs for time varying loading DFMX.
* The BFGS algorithm stopping criteria is set as the decrease of 
negative log-Likelihood function less than 1E3 * eps (eps: machine 
precision, which is 1.622*1E-16 on my machine). **AND** the norm of 
gradient vector is less than the norm of parameters. In this setup,
we are pursuing the *extreme* precision of estimation.


## 2.1 Likelihood change 

```python
====  ======   =======  ======      ======  ========  ======
..       dfm     dfmTV    diff        dfmX    dfmXTV    diff
====  ======   =======  ======      ======  ========  ======
XOM   170849    175721    4872      174363    179057    4694
INTC  151422    155933    4510      154819    160203    5383
UNH   155657    160427    4770      157956    163590    5634
DD    167804    174027    6222      171197    177630    6433
COF   134624    145016   10392      136855    147355   10500
====  ======   =======  ======      ======  ========  ======
```

## 2.2 Parameters
The table below reports the parameters of constant loading DFMX and 
time varying loading DFMX. 

```python 
dfmX 
====  ========  ========  =======  =======  ========  ========
        alpha0    alpha1    beta0    beta1    gamma0    gamma1
====  ========  ========  =======  =======  ========  ========
XOM     0.0026    1.0124  -0.0008   0.9339   -0.0047    0.8516
INTC    0.0037    1.0216   0.0000   1.0332    0.0039    0.8056
UNH     0.0047    1.0072   0.0005   0.9094    0.0004    0.8933
DD      0.0029    1.0143  -0.0006   0.9798   -0.0030    0.9430
COF     0.0108    0.9952  -0.0018   1.0623    0.0001    0.8389
====  ========  ========  =======  =======  ========  ========

====  ====   ======  ======================   ======================
              vD              mT                    mQ (x1E3) 
====  ====   ======  ======  ======  ======   ======  ======  ======  
mean  level  0.0007  0.9916  0.0862 -0.0148   0.8045 -0.2021  0.0482 
      term  -0.0000 -0.0006  0.9325  0.0016  -0.2021  0.0924 -0.0210 
      smile -0.0001  0.0024  0.0053  0.9744   0.0482 -0.0210  0.0863  
                                                               
std   level  0.0004  0.0083  0.0412  0.0208   0.3878  0.1085  0.0256 
      term   0.0001  0.0018  0.0161  0.0109   0.1085  0.0669  0.0327 
      smile  0.0002  0.0015  0.0127  0.0130   0.0256  0.0327  0.0726 
====  ====   ======  ======  ======  ======   ======  ======  ====== 


dfmXTV
====  ========  ========  =======  =======  ========  ========
..      alpha0    alpha1    beta0    beta1    gamma0    gamma1
====  ========  ========  =======  =======  ========  ========
XOM     0.0027    1.0106  -0.0003   0.9249    0.0019    0.8214
INTC    0.0035    1.0233  -0.0000   1.0472    0.0036    0.8803
UNH     0.0032    1.0140   0.0007   0.9416    0.0023    0.9054
DD      0.0028    1.0140  -0.0001   0.9577    0.0024    0.8932
COF     0.0030    1.0293  -0.0002   0.7655    0.0004    0.8976
====  ========  ========  =======  =======  ========  ========

====  ====   ======  ======================   ======================
              vD              mT                    mQ (x1E3) 
====  ====   ======  ======  ======  ======   ======  ======  ======  
mean  level  0.0006  0.9922  0.0761 -0.0251   0.8016 -0.1935  0.0612 
      term   0.0001 -0.0008  0.9405  0.0035  -0.1935  0.0859 -0.0098 
      smile -0.0003  0.0038  0.0048  0.9596   0.0612 -0.0098  0.1238 
                                                               
std   level  0.0004  0.0087  0.0369  0.0180   0.3641  0.1065  0.0294 
      term   0.0002  0.0021  0.0094  0.0079   0.1065  0.0647  0.0118 
      smile  0.0003  0.0026  0.0228  0.0336   0.0294  0.0118  0.1100 
====  ====   ======  ======  ======  ======   ======  ======  ====== 
```

## 2.3 DFMX factors 
The figure below plots the DFMX factors. The solid line is the 
constant DFMX factors, and the dash line is the time varying loading 
DFMX factors. I also plot the time varying DFM factors of SPX (which
is the exogenous variable in two-step DFMX setup).
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTSDhpVjMyNkZock0)


# 3. Variance Decomposition
The figure below plots th variance decomposition of time varying DFMX model.(The
reference click [here](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/vardecompDoc.png)).
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTSnMyZTFYNmY3V1E)

# 4. Fit 
## 4.1 On grid fit 
In this section, I plot the fit on grid, which is the fit of selected contracts.

Aggregate fit over stocks and models.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTLU1SUzhMSEtQdnc)

Fit of different IVS regions.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTMUUxNXNWQ05ubzg)

## 4.2 Off grid fit 
In this section, I calculate the fit of **ALL** available contracts (We 
train the models by selected contracts, and use parameters to calculate 
the fitted values of all available contracts). 

Aggregate off grid fit over stocks and models.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTNkhaUUEzQ25KblU)

Off grid fit of different regions of IVS.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTUEFwQWxiLUtWbEk)
