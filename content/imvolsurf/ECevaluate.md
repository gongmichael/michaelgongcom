# Economic Significance evaluation
I setup a option straddle strategy to evaluate the economic significance of DFM and DFMX 
models. Specifically:
* A buy (sell) signal is generated if the forecasted implied volatility increase (decrease)
by more than 1% (-1%).
* The least net cost of portfolio is set to 1$.
* No risk free rate is assumed (for days without trading, simply zero returns).

Besides, based on previous out-of-sample analysis, DFM and DFMX performs very bad in 
deep out of money put regions, I add some "focus" region to re-evaluate the trading 
returns, i.e., drop options outside the focus region. Below is the trading return plot.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTS1ozOVR5aTFGalE)
