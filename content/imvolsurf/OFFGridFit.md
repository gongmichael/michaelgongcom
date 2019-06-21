
# 1. In sample fit performance for off-grid contracts
This figure plots the in-sample RMSE for off-grid contracts on stock level. The right subplot normalize the RMSE of fully efficient DFM model to 1.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTNktuU1FISWU5VFk)



# 2. Out of sample forecasting performance for off-grid contracts

## 2.1 Stock Level
The table below reports the out-of-sample RMSE for off-grid contracts. Model with lowest RMSE is highlighted with `*`.
```python
________________________________________________
         fulDfm     ecoDfm    fulDfmX    ecoDfmX
________________________________________________
ticker
JNJ     0.045933   0.078299  0.045880*  0.078150
IBM     0.084223   0.111040  0.083951*  0.111044
PEP     0.057679*  0.083626  0.057842   0.083309
UTX     0.082849   0.110681  0.082679*  0.110456
UNP     0.088248*  0.104763  0.088262   0.104547
OXY     0.071886   0.090999  0.071696*  0.090750
COST    0.084082   0.105939  0.083944*  0.103390
EMC     0.131499*  0.158458  0.131504   0.158055
BK      0.118521   0.152506  0.117932*  0.151922
DVN     0.080291   0.100630  0.080252*  0.100561
________________________________________________
```

The table below reports the out-of-sample MCP for off-grid contracts. Model with hgihest MCP is highlighted with `*`.
```python
________________________________________________
         fulDfmM   ecoDfmM   fulDfmXM   ecoDfmXM
________________________________________________
ticker
JNJ     0.516890*  0.475930  0.516511   0.475807
IBM     0.470956   0.446953  0.472807*  0.446075
PEP     0.504182   0.484044  0.504819*  0.484453
UTX     0.501541   0.472095  0.503101*  0.472858
UNP     0.493533   0.468252  0.495180*  0.468769
OXY     0.511913*  0.492069  0.511890   0.493348
COST    0.484712   0.469654  0.485755*  0.466830
EMC     0.496390   0.478804  0.496894*  0.479092
BK      0.496446   0.461607  0.500818*  0.463646
DVN     0.513112   0.493093  0.513187*  0.492565
________________________________________________
```

## 2.2 Option groups Level
The figure below plots the out-of-sample forecasting RMSE grouped by maturity.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTbW1oT3RXVWliems)

The figure below plots the out-of-sample forecasting RMSE grouped by delta decile.
![](https://drive.google.com/uc?export=view&id=0B9DzYBQbrkqTNFVQWGhiekY3dHM)
