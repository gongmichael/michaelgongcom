# Data Description

## 7Firms.h
#### Created on 2016/11/03
* This file contains the screened data from 7FirmsInterpolated.csv and 7FirmsActual
* 7 Firms contain 'AAPL', 'LEG', 'MSFT', 'SPX', 'RHI', 'WMT', 'ZION'
* Below is the screening procedure
```python
# Exclude implied volatility above 70%
rawA = rawA[rawA['imvol'] < 1.0]
rawA.loc[(rawA['ticker'] == 'SPX') & (rawA['imvol'] > 0.7), 'imvol'] = np.nan
rawA.dropna(subset=['imvol'], how='any', inplace=True)

# Exclude price below 0.05 
rawA = rawA[rawA['best_offer'] > 0.05 ]

# Selection date and delta 
rawI = rawI[rawI['delta'].isin([0.20, 0.35, 0.50, -0.50, -0.35, -0.20])]
rawI = rawI[rawI['maturity'].isin([60, 122, 273, 547])]
rawA = rawA[(rawA['maturity'] <= 720) & (rawA['maturity'] >= 7)]
```
* A stands for actual data (real option transaction data), and I stands for Interpolation data

#### Subfile details
1. *rawA*: screened data for actual option contract 
2. *rawI*: screened data for Interpolation data 
3. *cleanA*: only keep relevant variable of actual option data for DFM
4. *cleanI*: only keep relevant variable of Interpolation data for DFM 
5. *finalA*: Selected contracts for missing groups

*This file is been compressed to 7firm.zip for archive*

----------
## 10Firms.h
#### Created on 2016/11/12
* As last discussed, firms in the bottom of S&P 500 might be too small. This time
we selected 10 stocks from S&P 100.
* The 10 stocks is selected based on market capitalization. The first stocks in 
each decile of S&P 100 is selected.
* As last discussed, only actual data is used for testing new economic restriced 
model.
* The data screening procedure is the same as *7Firms.h*. The only difference is 
we only keep contracts with maturity less than 365 days.


----------