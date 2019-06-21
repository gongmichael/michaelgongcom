# 1. Data Sources

In order to investigate the put call parity violation, the data comes from 3 dataset, all of them come from OptionMetrics.

1. Option Prices data
2. ZeroCoupon rate
3. Security prices


### 1.1 Zero Coupon rate overview
```python
       date  days      rate
0  20020104    11  1.869341
1  20020104    12  1.869925
2  20020104    47  1.899345
3  20020104    75  1.910800
4  20020104   103  1.942172
5  20020104   131  1.988259
6  20020104   166  2.049509
7  20020104   257  2.189949
8  20020104   348  2.404405
9  20020104   439  2.666471
```
I linearly (extra-)interpolate the zero coupon rate for maturities from 7 days to 240 days.

### 1.2 Security price
```python
    secid      date ticker      low     high     open    close   volume  return  shrout
0  101981  20020104     BK  42.2650  42.9800  42.4500  42.6100  1758300  0.0169  736464
1  101981  20020107     BK  43.0000  44.1500  43.9000  43.9500  3617800  0.0314  736464
2  101981  20020108     BK  42.9000  44.0000  43.7500  43.3300  2302400 -0.0141  736464
3  101981  20020109     BK  43.1000  44.2500  43.3300  43.4600  1867300  0.0030  736464
4  101981  20020110     BK  43.6500  45.7000  43.9000  45.3500  4951700  0.0435  736464
5  101981  20020111     BK  44.8000  46.5000  45.3500  44.9000  3099500 -0.0099  736464
6  101981  20020114     BK  43.7500  44.3700  44.3600  44.0100  3225500 -0.0198  736464
7  101981  20020115     BK  43.6000  44.8300  44.1000  43.7500  4052500 -0.0059  736464
8  101981  20020116     BK  42.7400  43.9000  43.5800  42.8400  2393100 -0.0165  736464
9  101981  20020117     BK  42.7500  43.5000  43.3400  42.7600  3056400 -0.0019  736464
```
I use the close price as security price.

# 2. Data Screen
Below is the number of observations remains after each filter for **Equity** options.

|Filters| Observations|
|-------|-------------|
|Original| 8,644,884|
|1.Excluding missing delta or imvol| 7,301,667|
|2.Excluding imvol above 1.0| 7,021,054|
|3.Exluding mid-quote below $0.05| 6,008,209|
|4.Exluding maturity outside [7,240] | 4,138,933| 

Applying the same filters to SPX index options, the index options has **4,007,858** observations. 
Therefore, after applying the 4 filters mentioned above, we have **8,146,791** observations remains.

# 3. New filters

### 3.1 Bid-Ask Spread
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTMGNTQUFJTU9zeDQ)

Zoom in the extreme out-of-money region:
```python
         Index call                     Equity call
______________________________    ___________________________
delta decile            spread    delta decile         spread
(-0.000442, 0.00235]    0.9913    (0.00418, 0.0147]    0.8668
(0.00235, 0.00336]      0.9728    (0.0147, 0.02]       0.7564
(0.00336, 0.00437]      0.9286    (0.02, 0.0248]       0.6908
(0.00437, 0.00535]      0.8966    (0.0248, 0.0294]     0.6618
(0.00535, 0.00636]      0.8791    (0.0294, 0.0338]     0.6390
(0.00636, 0.00748]      0.8707    (0.0338, 0.038]      0.6372
(0.00748, 0.00874]      0.8603    (0.038, 0.0422]      0.6322
(0.00874, 0.0102]       0.8373    (0.0422, 0.0465]     0.6144
(0.0102, 0.012]         0.7980    (0.0465, 0.0513]     0.5803
(0.012, 0.0142]         0.7540    (0.0513, 0.0564]     0.5496
______________________________    ___________________________

         Index Put                     Equity Put
______________________________    ___________________________
(-0.00344, -0.00307]    0.7271    (-0.0302, -0.0277]   0.4778
(-0.00307, -0.00272]    0.7408    (-0.0277, -0.0253]   0.4869
(-0.00272, -0.0024]     0.7476    (-0.0253, -0.0229]   0.4914
(-0.0024, -0.00208]     0.7507    (-0.0229, -0.0205]   0.4916
(-0.00208, -0.00179]    0.7636    (-0.0205, -0.0182]   0.4982
(-0.00179, -0.00151]    0.7743    (-0.0182, -0.0158]   0.5100
(-0.00151, -0.00126]    0.7910    (-0.0158, -0.0135]   0.5207
(-0.00126, -0.001]      0.8379    (-0.0135, -0.011]    0.5404
(-0.001, -0.000724]     0.8818    (-0.011, -0.0083]    0.5605
(-0.000724, -0.000168]  0.9230    (-0.0083, -0.00196]  0.5975
______________________________    ___________________________
```

As shown, I think excluding |delta|<0.01 for index option and |delta|<0.02 for equity options can supress the bid-ask spread to reasonable level.

|Filters| Observations|
|-------|-------------|
|Apply 4 filters| 8,146,791|
|Excluding extreme out-of-money option|7,402,859|

### 3.2 Put-call parity
In order to investigate the put call parity, we need to find the each put call pair for the same maturity and strike price.
Out of 7,402,859 contracts, I found 6,180,488 contracts that can paired with each other.

|Filters| Observations|
|-------|-------------|
|Apply 5 filters| 7,402,859|
|Excluding contracts that cannot be paired|6,180,488|

Below is a snapshot of paired dataset:
```python
  ticker       date   strike maturity      bid      ask   midquo   imvol   delta  optionid cp_flag
0     BK 2002-01-04  25.0000      197  17.5000  17.8000  17.6500  0.3568  0.9886  20522069       C
1     BK 2002-01-04  25.0000      197   0.1500   0.2500   0.2000  0.4344 -0.0332  20522076       P
2     BK 2002-01-04  30.0000      106  12.5000  12.8000  12.6500  0.3146  0.9877  20429450       C
3     BK 2002-01-04  30.0000      106   0.1500   0.3000   0.2250  0.4262 -0.0504  20429457       P
4     BK 2002-01-04  30.0000      169   9.5000   9.7000   9.6000  0.3329  0.9007  20502627       C
5     BK 2002-01-04  30.0000      169   0.3500   0.4500   0.4000  0.3234 -0.0914  20502630       P
```
**calculate the put call parity**
```python
parity['C-P'] = parity['ask']['C'] - parity['bid']['P']
parity['S-DK'] = parity['prc'] - parity['brate']*parity['strike']
parity['diff'] = abs((parity['C-P']-parity['S-DK'])/parity['S-DK'])
```
snapshot of the put call parity
```python
       ticker       date   strike maturity      bid              ask              C-P     S-DK    diff
cp_flag                                            C       P        C       P
0           BK 2002-01-04  25.0000      197  17.5000  0.1500  17.8000  0.2500  17.6500  18.1236  0.0261
1           BK 2002-01-04  30.0000      106  12.5000  0.1500  12.8000  0.3000  12.6500  13.1830  0.0404
2           BK 2002-01-04  30.0000      169   9.5000  0.3500   9.7000  0.4500   9.3500  13.2138  0.2924
3           BK 2002-01-04  30.0000      197  12.9000  0.4500  13.1000  0.5500  12.6500  13.2263  0.0436
4           BK 2002-01-04  32.5000       71   6.8000  0.1500   7.0000  0.2500   6.8500  10.7189  0.3609
5           BK 2002-01-04  32.5000      169   7.4000  0.7000   7.5000  0.8000   6.8000  10.7642  0.3683
6           BK 2002-01-04  35.0000       71   4.6000  0.4500   4.8000  0.5500   4.3500   8.2657  0.4737
7           BK 2002-01-04  35.0000      106   8.0000  0.6000   8.3000  0.7000   7.7000   8.2785  0.0699
8           BK 2002-01-04  35.0000      169   5.4000  1.2000   5.6000  1.3000   4.4000   8.3145  0.4708
9           BK 2002-01-04  35.0000      197   8.6000  1.2000   8.9000  1.3000   7.7000   8.3290  0.0755
```

Below is the plot the put-call parity difference against delta
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTMFhTdVp6bm5tenM)
The light grey dash line the 2.5. As shown, there are lot of speculations around ATM regions, and the violations of put-call parity is mostly severe in the ATM region. I set 2.5 as the threshold. If the difference of put call parity is larger than threshold, then drop the pair of contracts.

|Filters| Observations|
|-------|-------------|
|Apply 6 filters| 6,180,488|
|Excluding put call pairs with put-call parity difference larger than 2.5|6,040,856|
