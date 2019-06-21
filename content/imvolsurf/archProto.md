---
title: "Stock Summary"
date: 2018-09-12T14:51:12+06:00
author: Michael
description: Uses Async call to lucene index for super fast autocompletion to address performance issue loading config.
featured: true
categories: ["cat1"]
tags: ["java","jQuery"]
---

# Fully Efficient DFM with GARCH(1,1) prototype (JNJ as illustration)

**Model Documentation**
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTLVZNS2hsYnlmT28)
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTWnByYmpSNlJYb1k)


# 0. Parameters
GARCH parameters
```python
'omega': 0.0001 (fixed),   'alpha':0.75335567195082032,  'beta': 0.24459914836890723
```

Loading for latent error factor
```python
DOTM Put 1  [ 0.34593] 
DOTM Put 2  [ 0.09793] 
DOTM Put 3  [ 0.06754] 
OTM Put 1   [ 0.25441] 
OTM Put 2   [ 0.08145] 
OTM Put 3   [ 0.03036] 
ATM Put 1   [ 0.20045] 
ATM Put 2   [ 0.07   ] 
ATM Put 3   [ 0.02845] 
ATM Call 1  [ 0.22505] 
ATM Call 2  [ 0.0936 ] 
ATM Call 3  [ 0.06643] 
OTM Call 1  [ 0.21288] 
OTM Call 2  [ 0.08585] 
OTM Call 3  [ 0.0696 ] 
DOTM Call 1 [ 0.22998] 
DOTM Call 2 [ 0.09105] 
DOTM Call 3 [ 0.08369]
```

# 1. Factors
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTVzFNbERGTUo5Y2c)
# 2. Fit
Likelihood
```python
'dfm-garch': 189593.08390802974, 'dfm':186762.2830423559
```


Below is the in-sample RMSE at stock level (JNJ)
```python
'dfm-garch': 0.014111563467550176, 'dfm': 0.015267540398543699
```
Below is the in-sample RMSE at option group level.
```python
                dfm   dfm-garch   change(%)
DOTM Put 1   0.037270  0.034163  -8.337114
DOTM Put 2   0.020157  0.020585   2.122947
DOTM Put 3   0.013198  0.008222 -37.703092
OTM Put 1    0.018669  0.017206  -7.839344
OTM Put 2    0.010029  0.009605  -4.222386
OTM Put 3    0.006936  0.006561  -5.398216
ATM Put 1    0.015056  0.014808  -1.644943
ATM Put 2    0.008800  0.007874 -10.516285
ATM Put 3    0.007159  0.006478  -9.517520
ATM Call 1   0.011826  0.008853 -25.133113
ATM Call 2   0.006519  0.006323  -3.015472
ATM Call 3   0.005256  0.005468   4.032168
OTM Call 1   0.011546  0.008785 -23.910682
OTM Call 2   0.004930  0.004686  -4.950154
OTM Call 3   0.002777  0.003008   8.300271
DOTM Call 1  0.028989  0.027624  -4.710728
DOTM Call 2  0.011622  0.011708   0.742297
DOTM Call 3  0.004782  0.003848 -19.531603
```
# 3. Residuals 

## 3.1 Test for Heteroskedasticity (Engle's Test (lag:5))
```python
                 dfm  dfm-garch
DOTM Put 1   1118.02  1037.76
DOTM Put 2    927.66  1006.00
DOTM Put 3   1352.34   604.31
OTM Put 1     368.69   331.49
OTM Put 2     815.47   861.52
OTM Put 3     750.28   835.02
ATM Put 1     359.93   305.38
ATM Put 2     351.75   375.46
ATM Put 3     464.19   443.61
ATM Call 1    602.14   442.56
ATM Call 2   1013.38  1008.86
ATM Call 3    238.44   224.34
OTM Call 1     58.01    32.65
OTM Call 2    1225.8   945.09
OTM Call 3    316.92   350.64
DOTM Call 1     37.2    37.06
DOTM Call 2   486.26   489.04
DOTM Call 3   840.06   448.54
```
## 3.1 Test for Autocorrelation (Ljung–Box test (lag:5))
```python
                 dfm  dfm-garch
DOTM Put 1   2748.78  1369.93
DOTM Put 2   4704.91  4924.51
DOTM Put 3   5789.49   705.59
OTM Put 1    2381.74  1227.54
OTM Put 2    1733.34  1397.79
OTM Put 3    2040.99  2015.29
ATM Put 1    2270.62  1643.74
ATM Put 2    2124.54  1175.75
ATM Put 3    3908.91  3096.55
ATM Call 1   2114.04  1216.28
ATM Call 2   2560.43  1857.39
ATM Call 3    2082.1   2332.1
OTM Call 1   2154.95  1357.62
OTM Call 2    3853.9  2800.13
OTM Call 3   1169.82  1696.46
DOTM Call 1   854.23   625.87
DOTM Call 2  4077.69  4171.95
DOTM Call 3  6003.52  2603.96
```