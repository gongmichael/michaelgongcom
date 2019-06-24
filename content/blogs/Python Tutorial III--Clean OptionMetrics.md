---
title: "Python Tutorial Part III--Clean OptionMetrics"
date: 2017-04-07T12:52:36+06:00
image: images/blog/blog-post-1.jpg
author: Michael Gong
featured: true
thumbnail: "/img/posts/PyTutorial3/OptionMetrics.png"
tags: ["Python","Tutorial","OptionMetrics"]
---

# 1. Clean OptionMetrics dataset


```python
import pandas as pd 
import numpy as np 
import matplotlib.pyplot as plt 
%matplotlib inline
```


```python
raw = pd.read_csv('5Firms.csv.zip',parse_dates=[2,3])
raw.drop(columns=['secid','issuer','exercise_style','last_date'],inplace=True)
raw.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>exdate</th>
      <th>cp_flag</th>
      <th>strike_price</th>
      <th>best_bid</th>
      <th>best_offer</th>
      <th>volume</th>
      <th>open_interest</th>
      <th>imvol</th>
      <th>delta</th>
      <th>optionid</th>
      <th>ticker</th>
      <th>index_flag</th>
      <th>midquo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>34.000</td>
      <td>9.300</td>
      <td>10.250</td>
      <td>0</td>
      <td>0</td>
      <td>0.529</td>
      <td>0.988</td>
      <td>70433772</td>
      <td>COF</td>
      <td>0</td>
      <td>9.775</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>35.000</td>
      <td>8.600</td>
      <td>9.200</td>
      <td>0</td>
      <td>1237</td>
      <td>0.666</td>
      <td>0.948</td>
      <td>45658102</td>
      <td>COF</td>
      <td>0</td>
      <td>8.900</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>36.000</td>
      <td>7.350</td>
      <td>8.300</td>
      <td>0</td>
      <td>10</td>
      <td>0.515</td>
      <td>0.965</td>
      <td>67966062</td>
      <td>COF</td>
      <td>0</td>
      <td>7.825</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>37.000</td>
      <td>6.800</td>
      <td>7.050</td>
      <td>0</td>
      <td>4</td>
      <td>0.550</td>
      <td>0.929</td>
      <td>70433773</td>
      <td>COF</td>
      <td>0</td>
      <td>6.925</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>38.000</td>
      <td>5.850</td>
      <td>6.000</td>
      <td>0</td>
      <td>589</td>
      <td>0.481</td>
      <td>0.921</td>
      <td>54279804</td>
      <td>COF</td>
      <td>0</td>
      <td>5.925</td>
    </tr>
  </tbody>
</table>
</div>




```python
raw.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 4467804 entries, 14 to 5719675
    Data columns (total 14 columns):
    date             datetime64[ns]
    exdate           datetime64[ns]
    cp_flag          object
    strike_price     float64
    best_bid         float64
    best_offer       float64
    volume           int64
    open_interest    int64
    imvol            float64
    delta            float64
    optionid         int64
    ticker           object
    index_flag       int64
    midquo           float64
    dtypes: datetime64[ns](2), float64(6), int64(4), object(2)
    memory usage: 511.3+ MB



```python
# change the implied volatility name (it's too long)
raw.rename(columns={'impl_volatility': 'imvol'}, inplace=True)
# transform the strike price and calculate the mid quotes
raw['strike_price'] = raw['strike_price'] / 1000.0
raw['midquo'] = (raw['best_bid'] + raw['best_offer']) / 2
raw.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>exdate</th>
      <th>cp_flag</th>
      <th>strike_price</th>
      <th>best_bid</th>
      <th>best_offer</th>
      <th>volume</th>
      <th>open_interest</th>
      <th>imvol</th>
      <th>delta</th>
      <th>optionid</th>
      <th>ticker</th>
      <th>index_flag</th>
      <th>midquo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.034</td>
      <td>9.300</td>
      <td>10.250</td>
      <td>0</td>
      <td>0</td>
      <td>0.529</td>
      <td>0.988</td>
      <td>70433772</td>
      <td>COF</td>
      <td>0</td>
      <td>9.775</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.035</td>
      <td>8.600</td>
      <td>9.200</td>
      <td>0</td>
      <td>1237</td>
      <td>0.666</td>
      <td>0.948</td>
      <td>45658102</td>
      <td>COF</td>
      <td>0</td>
      <td>8.900</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.036</td>
      <td>7.350</td>
      <td>8.300</td>
      <td>0</td>
      <td>10</td>
      <td>0.515</td>
      <td>0.965</td>
      <td>67966062</td>
      <td>COF</td>
      <td>0</td>
      <td>7.825</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.037</td>
      <td>6.800</td>
      <td>7.050</td>
      <td>0</td>
      <td>4</td>
      <td>0.550</td>
      <td>0.929</td>
      <td>70433773</td>
      <td>COF</td>
      <td>0</td>
      <td>6.925</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.038</td>
      <td>5.850</td>
      <td>6.000</td>
      <td>0</td>
      <td>589</td>
      <td>0.481</td>
      <td>0.921</td>
      <td>54279804</td>
      <td>COF</td>
      <td>0</td>
      <td>5.925</td>
    </tr>
  </tbody>
</table>
</div>




```python
# drop contracts with NaN implied volatility or Nan delta
raw.dropna(subset=['imvol'], how='any', inplace=True)
raw.dropna(subset=['delta'], how='any', inplace=True)
```


```python
# Exclude implied volatility above 150%
raw = raw[raw['imvol'] < 1.5]
# Exclude price below 0.05
raw = raw[raw['best_offer'] > 0.05]
raw.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>exdate</th>
      <th>cp_flag</th>
      <th>strike_price</th>
      <th>best_bid</th>
      <th>best_offer</th>
      <th>volume</th>
      <th>open_interest</th>
      <th>imvol</th>
      <th>delta</th>
      <th>optionid</th>
      <th>ticker</th>
      <th>index_flag</th>
      <th>midquo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.034</td>
      <td>9.300</td>
      <td>10.250</td>
      <td>0</td>
      <td>0</td>
      <td>0.529</td>
      <td>0.988</td>
      <td>70433772</td>
      <td>COF</td>
      <td>0</td>
      <td>9.775</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.035</td>
      <td>8.600</td>
      <td>9.200</td>
      <td>0</td>
      <td>1237</td>
      <td>0.666</td>
      <td>0.948</td>
      <td>45658102</td>
      <td>COF</td>
      <td>0</td>
      <td>8.900</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.036</td>
      <td>7.350</td>
      <td>8.300</td>
      <td>0</td>
      <td>10</td>
      <td>0.515</td>
      <td>0.965</td>
      <td>67966062</td>
      <td>COF</td>
      <td>0</td>
      <td>7.825</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.037</td>
      <td>6.800</td>
      <td>7.050</td>
      <td>0</td>
      <td>4</td>
      <td>0.550</td>
      <td>0.929</td>
      <td>70433773</td>
      <td>COF</td>
      <td>0</td>
      <td>6.925</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.038</td>
      <td>5.850</td>
      <td>6.000</td>
      <td>0</td>
      <td>589</td>
      <td>0.481</td>
      <td>0.921</td>
      <td>54279804</td>
      <td>COF</td>
      <td>0</td>
      <td>5.925</td>
    </tr>
  </tbody>
</table>
</div>




```python
# calculate maturity
raw['maturity'] = ((raw['exdate'] - raw['date']) / np.timedelta64(1, 'D')).astype(int)
# Exclude contracts with maturity longer than 240 days and shorter than 7 days
raw = raw[(raw['maturity'] <= 360) & (raw['maturity'] >= 7)]
raw.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>exdate</th>
      <th>cp_flag</th>
      <th>strike_price</th>
      <th>best_bid</th>
      <th>best_offer</th>
      <th>volume</th>
      <th>open_interest</th>
      <th>imvol</th>
      <th>delta</th>
      <th>optionid</th>
      <th>ticker</th>
      <th>index_flag</th>
      <th>midquo</th>
      <th>maturity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.034</td>
      <td>9.300</td>
      <td>10.250</td>
      <td>0</td>
      <td>0</td>
      <td>0.529</td>
      <td>0.988</td>
      <td>70433772</td>
      <td>COF</td>
      <td>0</td>
      <td>9.775</td>
      <td>18</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.035</td>
      <td>8.600</td>
      <td>9.200</td>
      <td>0</td>
      <td>1237</td>
      <td>0.666</td>
      <td>0.948</td>
      <td>45658102</td>
      <td>COF</td>
      <td>0</td>
      <td>8.900</td>
      <td>18</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.036</td>
      <td>7.350</td>
      <td>8.300</td>
      <td>0</td>
      <td>10</td>
      <td>0.515</td>
      <td>0.965</td>
      <td>67966062</td>
      <td>COF</td>
      <td>0</td>
      <td>7.825</td>
      <td>18</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.037</td>
      <td>6.800</td>
      <td>7.050</td>
      <td>0</td>
      <td>4</td>
      <td>0.550</td>
      <td>0.929</td>
      <td>70433773</td>
      <td>COF</td>
      <td>0</td>
      <td>6.925</td>
      <td>18</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2012-01-03</td>
      <td>2012-01-21</td>
      <td>C</td>
      <td>0.038</td>
      <td>5.850</td>
      <td>6.000</td>
      <td>0</td>
      <td>589</td>
      <td>0.481</td>
      <td>0.921</td>
      <td>54279804</td>
      <td>COF</td>
      <td>0</td>
      <td>5.925</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# save the cleaned raw data, because it is time-consuming to processing, next time we just need to load it
hdf = pd.HDFStore('hdf.h5')
hdf['raw'] = raw
hdf.close()
```


```python
# if next time we start from here
# hdf = pd.HDFStore('hdf.h5')
# raw = hdf['raw']
# hdf.close()
```

# 2. Some summary statistics


```python
# select variable we want 
clean = raw[['date', 'ticker', 'imvol', 'delta', 'maturity', 'cp_flag']].reset_index(drop=True)
clean.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>ticker</th>
      <th>imvol</th>
      <th>delta</th>
      <th>maturity</th>
      <th>cp_flag</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.529</td>
      <td>0.988</td>
      <td>18</td>
      <td>C</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.666</td>
      <td>0.948</td>
      <td>18</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.515</td>
      <td>0.965</td>
      <td>18</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.550</td>
      <td>0.929</td>
      <td>18</td>
      <td>C</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.481</td>
      <td>0.921</td>
      <td>18</td>
      <td>C</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('stocks ticker:\n',clean['ticker'].unique())
print('maturity range:\n',clean['maturity'].min(),clean['maturity'].max())
print('delta range:\n',clean['delta'].min(),clean['delta'].max())
```

    stocks ticker:
     ['COF' 'DD' 'XOM' 'INTC' 'SPX' 'UNH']
    maturity range:
     7 360
    delta range:
     -0.999934 0.999999



```python
stats = clean.groupby(['ticker','cp_flag'])[['imvol','delta']].describe()
stats
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="8" halign="left">imvol</th>
      <th colspan="8" halign="left">delta</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>ticker</th>
      <th>cp_flag</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">COF</th>
      <th>C</th>
      <td>100419.000</td>
      <td>0.295</td>
      <td>0.146</td>
      <td>0.063</td>
      <td>0.199</td>
      <td>0.249</td>
      <td>0.341</td>
      <td>1.000</td>
      <td>100419.000</td>
      <td>0.523</td>
      <td>0.359</td>
      <td>0.008</td>
      <td>0.130</td>
      <td>0.577</td>
      <td>0.884</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>P</th>
      <td>108812.000</td>
      <td>0.319</td>
      <td>0.144</td>
      <td>0.057</td>
      <td>0.213</td>
      <td>0.280</td>
      <td>0.386</td>
      <td>1.000</td>
      <td>108812.000</td>
      <td>-0.372</td>
      <td>0.340</td>
      <td>-1.000</td>
      <td>-0.710</td>
      <td>-0.240</td>
      <td>-0.059</td>
      <td>-0.003</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">DD</th>
      <th>C</th>
      <td>125769.000</td>
      <td>0.264</td>
      <td>0.138</td>
      <td>0.063</td>
      <td>0.178</td>
      <td>0.217</td>
      <td>0.299</td>
      <td>0.999</td>
      <td>125769.000</td>
      <td>0.553</td>
      <td>0.366</td>
      <td>0.008</td>
      <td>0.146</td>
      <td>0.639</td>
      <td>0.917</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>P</th>
      <td>134941.000</td>
      <td>0.262</td>
      <td>0.105</td>
      <td>0.047</td>
      <td>0.187</td>
      <td>0.233</td>
      <td>0.313</td>
      <td>0.999</td>
      <td>134941.000</td>
      <td>-0.392</td>
      <td>0.352</td>
      <td>-1.000</td>
      <td>-0.760</td>
      <td>-0.255</td>
      <td>-0.063</td>
      <td>-0.004</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">INTC</th>
      <th>C</th>
      <td>113863.000</td>
      <td>0.306</td>
      <td>0.150</td>
      <td>0.084</td>
      <td>0.218</td>
      <td>0.248</td>
      <td>0.331</td>
      <td>1.000</td>
      <td>113863.000</td>
      <td>0.577</td>
      <td>0.349</td>
      <td>0.015</td>
      <td>0.209</td>
      <td>0.660</td>
      <td>0.922</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>P</th>
      <td>127602.000</td>
      <td>0.293</td>
      <td>0.113</td>
      <td>0.074</td>
      <td>0.221</td>
      <td>0.260</td>
      <td>0.334</td>
      <td>1.000</td>
      <td>127602.000</td>
      <td>-0.505</td>
      <td>0.359</td>
      <td>-1.000</td>
      <td>-0.882</td>
      <td>-0.514</td>
      <td>-0.122</td>
      <td>-0.007</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">SPX</th>
      <th>C</th>
      <td>1240711.000</td>
      <td>0.245</td>
      <td>0.151</td>
      <td>0.050</td>
      <td>0.139</td>
      <td>0.202</td>
      <td>0.297</td>
      <td>1.000</td>
      <td>1240711.000</td>
      <td>0.674</td>
      <td>0.361</td>
      <td>0.001</td>
      <td>0.373</td>
      <td>0.869</td>
      <td>0.970</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>P</th>
      <td>1344534.000</td>
      <td>0.259</td>
      <td>0.135</td>
      <td>0.033</td>
      <td>0.157</td>
      <td>0.233</td>
      <td>0.326</td>
      <td>1.000</td>
      <td>1344534.000</td>
      <td>-0.251</td>
      <td>0.328</td>
      <td>-1.000</td>
      <td>-0.419</td>
      <td>-0.066</td>
      <td>-0.010</td>
      <td>-0.000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">UNH</th>
      <th>C</th>
      <td>86588.000</td>
      <td>0.280</td>
      <td>0.133</td>
      <td>0.076</td>
      <td>0.204</td>
      <td>0.236</td>
      <td>0.303</td>
      <td>1.000</td>
      <td>86588.000</td>
      <td>0.536</td>
      <td>0.364</td>
      <td>0.007</td>
      <td>0.141</td>
      <td>0.600</td>
      <td>0.904</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>P</th>
      <td>92393.000</td>
      <td>0.290</td>
      <td>0.118</td>
      <td>0.066</td>
      <td>0.213</td>
      <td>0.255</td>
      <td>0.333</td>
      <td>0.999</td>
      <td>92393.000</td>
      <td>-0.395</td>
      <td>0.352</td>
      <td>-1.000</td>
      <td>-0.759</td>
      <td>-0.271</td>
      <td>-0.058</td>
      <td>-0.003</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">XOM</th>
      <th>C</th>
      <td>112332.000</td>
      <td>0.240</td>
      <td>0.140</td>
      <td>0.034</td>
      <td>0.156</td>
      <td>0.187</td>
      <td>0.265</td>
      <td>1.000</td>
      <td>112332.000</td>
      <td>0.535</td>
      <td>0.358</td>
      <td>0.006</td>
      <td>0.148</td>
      <td>0.601</td>
      <td>0.889</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>P</th>
      <td>126483.000</td>
      <td>0.244</td>
      <td>0.110</td>
      <td>0.039</td>
      <td>0.169</td>
      <td>0.213</td>
      <td>0.287</td>
      <td>0.999</td>
      <td>126483.000</td>
      <td>-0.418</td>
      <td>0.353</td>
      <td>-1.000</td>
      <td>-0.796</td>
      <td>-0.316</td>
      <td>-0.073</td>
      <td>-0.003</td>
    </tr>
  </tbody>
</table>
</div>



# 3. Transform regression variables


```python
# constant
clean['const'] = 1
# scaled moneyness
clean['delta_n'] = clean['delta']
clean.loc[(clean['cp_flag']=='P'),'delta_n'] = 1 + clean.loc[(clean['cp_flag']=='P'),'delta']
clean['delta_n'] = clean['delta_n'] - 0.5
# scaled maturity
clean['matur_n'] = clean['maturity']/360 - 0.5
# quadratic term of moneyness
clean['delta2'] = clean['delta_n']**2
# interaction between maturity and moneyness
clean['inter'] = clean['delta_n']*clean['matur_n']
clean.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>ticker</th>
      <th>imvol</th>
      <th>delta</th>
      <th>maturity</th>
      <th>cp_flag</th>
      <th>const</th>
      <th>delta_n</th>
      <th>matur_n</th>
      <th>delta2</th>
      <th>inter</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.529</td>
      <td>0.988</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.488</td>
      <td>-0.450</td>
      <td>0.239</td>
      <td>-0.220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.666</td>
      <td>0.948</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.448</td>
      <td>-0.450</td>
      <td>0.201</td>
      <td>-0.202</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.515</td>
      <td>0.965</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.465</td>
      <td>-0.450</td>
      <td>0.216</td>
      <td>-0.209</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.550</td>
      <td>0.929</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.429</td>
      <td>-0.450</td>
      <td>0.184</td>
      <td>-0.193</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.481</td>
      <td>0.921</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.421</td>
      <td>-0.450</td>
      <td>0.177</td>
      <td>-0.189</td>
    </tr>
  </tbody>
</table>
</div>



# 4. Cross-section regression


```python
import statsmodels.api as sm
```

## 4.1 straightforward way


```python
tickers = clean['ticker'].unique()
ticker = tickers[0]

tmp = clean[clean['ticker']==ticker]
dates = tmp['date'].unique()
tmp = tmp[tmp['date']==dates[0]]
tmp.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>ticker</th>
      <th>imvol</th>
      <th>delta</th>
      <th>maturity</th>
      <th>cp_flag</th>
      <th>const</th>
      <th>delta_n</th>
      <th>matur_n</th>
      <th>delta2</th>
      <th>inter</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.529</td>
      <td>0.988</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.488</td>
      <td>-0.450</td>
      <td>0.239</td>
      <td>-0.220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.666</td>
      <td>0.948</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.448</td>
      <td>-0.450</td>
      <td>0.201</td>
      <td>-0.202</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.515</td>
      <td>0.965</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.465</td>
      <td>-0.450</td>
      <td>0.216</td>
      <td>-0.209</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.550</td>
      <td>0.929</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.429</td>
      <td>-0.450</td>
      <td>0.184</td>
      <td>-0.193</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012-01-03</td>
      <td>COF</td>
      <td>0.481</td>
      <td>0.921</td>
      <td>18</td>
      <td>C</td>
      <td>1</td>
      <td>0.421</td>
      <td>-0.450</td>
      <td>0.177</td>
      <td>-0.189</td>
    </tr>
  </tbody>
</table>
</div>




```python
exogs = ['const','delta_n','matur_n','delta2','inter']
mB = np.linalg.lstsq(tmp[exogs],tmp['imvol'])[0]
```


```python
mB
```




    array([     0.272,      0.176,     -0.170,      0.920,      0.054])



## 4.2 More concise way


```python
def CrossReg(df,exogs):
    mod = sm.OLS(df['imvol'],df[exogs]).fit()
    ind = pd.MultiIndex.from_product([['params','tvalues'],exogs])
    ret = pd.Series(index=ind)
    ret.loc['params'] = mod.params.values
    ret.loc['tvalues'] = mod.tvalues.values
    ret.loc['r2'] = mod.rsquared
    return ret 

exogs = ['const','delta_n','matur_n','delta2','inter']
mB = clean.groupby(['ticker','date']).apply(lambda x:CrossReg(x,exogs))
```


```python
mB.loc['SPX']['params'].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f2d109103c8>




![png](/img/posts/PyTutorial3/output_24_1.png)


## 4.3 Need for speed

### 4.3.1 Just for coefficients, sequential


```python
def CrossRegSpeed(df,exogs):
    mB = np.linalg.lstsq(df[exogs],df['imvol'])[0]
    return pd.Series(mB,index=exogs)
mB = clean.groupby(['ticker','date']).apply(lambda x:CrossRegSpeed(x,exogs))
```

### 4.3.2 Just for coefficients, parallel


```python
import multiprocessing as mp
from functools import partial

def CrossRegParHelper(ticker):
    tmp = DATA[DATA['ticker']==ticker]
    mB = tmp.groupby(['date']).apply(lambda x:CrossRegSpeed(x,EXOGS))
    mB['ticker'] = ticker
    return mB

def CrossRegPar(data,exogs):
    global DATA, EXOGS
    DATA = data 
    EXOGS = exogs
    pool = mp.Pool(processes=12)
    results = pool.map(CrossRegParHelper, tickers)
    pool.close()
    pool.join()
    mB = pd.concat(results)
    return mB

mB = CrossRegPar(clean,exogs)
```


```python
%timeit clean.groupby(['ticker','date']).apply(lambda x:CrossRegSpeed(x,exogs))
```

    8 s ± 33.4 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)



```python
%timeit CrossRegPar(clean,exogs)
```
    4.2 s ± 51.4 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)


### 4.3.3 JIT compilation


```python
import numba as nb
```


```python
clean.sort_values(by=['ticker','date'],inplace=True)
locs = clean.groupby(['ticker','date']).size().values.cumsum()
locs = np.insert(locs,0,0)
mY = clean[['imvol']].values
mX = clean[exogs].values
```


```python
@nb.jit
def CrossRegJIT(mX,mY,locs):
    N = locs.shape[0]
    mB = np.zeros((mX.shape[1],N-1))
    for i in range(0,N-1):
        b = np.linalg.lstsq(mX[locs[i]:locs[i+1]],mY[locs[i]:locs[i+1]])[0]
        mB[:,[i]] = b
    return mB

mB = CrossRegJIT(mX,mY,locs)
```


```python
%timeit CrossRegJIT(mX,mY,locs)
```

    702 ms ± 6.65 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)



```python
mB = pd.DataFrame(mB.T,index=clean.groupby(['ticker','date']).size().index,columns=exogs)
```


```python
mB.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>const</th>
      <th>delta_n</th>
      <th>matur_n</th>
      <th>delta2</th>
      <th>inter</th>
      <th>ticker</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-03</th>
      <td>0.272</td>
      <td>0.176</td>
      <td>-0.170</td>
      <td>0.920</td>
      <td>0.054</td>
      <td>COF</td>
    </tr>
    <tr>
      <th>2012-01-04</th>
      <td>0.288</td>
      <td>0.079</td>
      <td>-0.090</td>
      <td>1.023</td>
      <td>-0.263</td>
      <td>COF</td>
    </tr>
    <tr>
      <th>2012-01-05</th>
      <td>0.288</td>
      <td>0.160</td>
      <td>-0.129</td>
      <td>0.776</td>
      <td>-0.110</td>
      <td>COF</td>
    </tr>
    <tr>
      <th>2012-01-06</th>
      <td>0.251</td>
      <td>0.246</td>
      <td>-0.149</td>
      <td>1.189</td>
      <td>0.076</td>
      <td>COF</td>
    </tr>
    <tr>
      <th>2012-01-09</th>
      <td>0.267</td>
      <td>0.176</td>
      <td>-0.167</td>
      <td>0.881</td>
      <td>0.312</td>
      <td>COF</td>
    </tr>
  </tbody>
</table>
</div>



# 5. Principal Component Analysis


```python
mB.reset_index(inplace=True)
level = pd.pivot_table(mB,index='date',columns='ticker',values='const')
```


```python
level.plot()
```

![png](/img/posts/PyTutorial3/output_41_1.png)



```python
from sklearn import preprocessing
import scipy as sp
def pcaAnalysis(mX, iPC, useCov=1):
    if useCov:
        # mCov = np.cov(mX, rowvar = 0)
        de_meanX = mX - np.tile(np.mean(mX, axis=0), [mX.shape[0], 1])
        mCov = np.dot(de_meanX.transpose(), de_meanX) / mX.shape[0]

        eigval, eigvec = sp.linalg.eigh(mCov)
        eigidx = np.argsort(eigval)
        maxidx = eigidx[::-1]
        sorted_vec = eigvec[:, maxidx]

        mPC = np.dot(de_meanX, sorted_vec)
        return mPC[:, 0:iPC], (eigval[maxidx] / np.sum(eigval))[0:iPC]

    else:
        mCoef = np.corrcoef(mX, rowvar=0)
        scaled_X = preprocessing.scale(mX)
        eigval, eigvec = sp.linalg.eigh(mCoef)

        eigidx = np.argsort(eigval)
        maxidx = eigidx[::-1]
        sorted_vec = eigvec[:, maxidx]

        mPC = np.dot(scaled_X, sorted_vec)
        return preprocessing.scale(mPC[:, 0:iPC]), (eigval[maxidx] /
                                                    np.sum(eigval))[0:iPC]
```


```python
mPC,pct = pcaAnalysis(level.values[:500],3)
pct
```




    array([  7.22e-01,   9.53e-02,   6.82e-02])




```python
plt.plot(mPC)
```




    [<matplotlib.lines.Line2D at 0x7f054028cb70>,
     <matplotlib.lines.Line2D at 0x7f054028c518>,
     <matplotlib.lines.Line2D at 0x7f054028cf28>]




![png](/img/posts/PyTutorial3/output_44_1.png)
