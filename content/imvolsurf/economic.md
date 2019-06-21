# Straddle strategy
In this version, I only implement the option straddle strategy to evaluate
the economic significance of DFM and DFMX models.

A trade is generated only if 
* Forecasted implied volatility increases (decreases) by more than 1% (-1%).
* The option portfolio costs (gains in short position) at least 1$ to avoid
extreme portfolio 

Transaction costs is considered by the best bid price and best offer price.
For option predicted to buy (sell), I look for the best offer (bid) price, then in the 
next day I look for the best bid (offer) price to sell (buy) it. A value of 1000$
option portfolio is invested and re-balanced on daily basis.

If excluding the transaction costs, I simply take the mid-quote as option 
price for buy and sell. Note that in my thesis I only consider ATM options,
in this version I consider all the available options.

```python 
Daily return without transaction costs 

             DFM      DFMX     DFMX3i
ticker                               
AAPL    0.021578  0.001035   0.026916
CL      0.016368  0.018658   0.023016
CMCSA   0.014185  0.024596   0.016328
CVX     0.012849  0.011086   0.021563
DIS     0.016398  0.014212   0.014172
DOW     0.010720  0.012822   0.010764
FDX     0.024536  0.022324   0.009712
LLY     0.020318  0.011282   0.018278
MO      0.015462  0.008491   0.013134
NSC     0.016170  0.004784   0.022691

Daily return with transaction costs 

             DFM      DFMX     DFMX3i
ticker                               
AAPL   -0.122029 -0.126755  -0.109209
CL     -0.331004 -0.278153  -0.327702
CMCSA  -0.262986 -0.256619  -0.251219
CVX    -0.215189 -0.206948  -0.190050
DIS    -0.237475 -0.222399  -0.234552
DOW    -0.238421 -0.193924  -0.237525
FDX    -0.249230 -0.196694  -0.242553
LLY    -0.301601 -0.343404  -0.303037
MO     -0.217625 -0.193722  -0.210372
NSC    -0.344849 -0.330743  -0.315905
```