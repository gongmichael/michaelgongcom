# Out-of-sample performace evaluation
* I estimated 4 models: fully efficient DFM (fulDfm), economic restricted DFM (ecoDfm), fully efficient DFMX(fulDfmX), and economic restricted DFMX (ecoDfmX).
* The window size is 1000 days. I re-estimated models every 10 days (approx. 2 weeks).
* The estimation procedure is pushed to extreme: BFGS stops when the decreases of negative likelihood less than the machine precision (1E-16).


## 1. On stock level
The table below reports the RMSE at stock level. The best model with lowest RMSE is highlighted with `*`.
```python
       ecoDfm  ecoDfmX    fulDfm   fulDfmX
JNJ   0.03051  0.03034  0.01731*   0.01736
IBM   0.03150  0.03153  0.01776*   0.01786
PEP   0.02764  0.02745  0.01629*   0.01630
UTX   0.03595  0.03585  0.01816*   0.01822
UNP   0.03053  0.03052  0.01884*   0.01887
OXY   0.03562  0.03551  0.02341*   0.02342
COST  0.02789  0.02679  0.01749*   0.01760
EMC   0.04210  0.04199  0.03122*   0.03123
BK    0.05065  0.05060  0.03505    0.03500*
DVN   0.03263  0.03267  0.02247    0.02246*
```

The table below reports the MCP at stock level. The best model with highest MCP is highlighted with `*`.
```python
       ecoDfm  ecoDfmX    fulDfm   fulDfmX
JNJ   0.53310  0.53319  0.56771*   0.56741
IBM   0.52362  0.52277  0.55692*   0.55590
PEP   0.53472  0.53445  0.57646*   0.57562
UTX   0.52777  0.52993  0.57477    0.57651*
UNP   0.53180  0.53292  0.57300    0.57304*
OXY   0.53242  0.53276  0.56182*   0.55907
COST  0.53438  0.52911  0.56862*   0.56734
EMC   0.54192  0.53955  0.57083    0.57126*
BK    0.53433  0.53342  0.57904    0.58134*
DVN   0.53714  0.53691  0.56046*   0.55961
```


## 2. On option groups level 
The table below shows the RMSE at option groups level. The number of times that the model outperforms is shown in parenthesis (we have 10 stocks in total).

```python
                ecoDfm    ecoDfmX     fulDfm    fulDfmX
DOTM Put 1   0.0688(0)  0.0684(0)  0.0425(2)   0.0423(8)*
DOTM Put 2   0.0656(0)  0.0650(0)  0.0258(1)   0.0256(9)*
DOTM Put 3   0.0680(0)  0.0675(0)  0.0212(5)   0.0212(5)*
OTM Put 1    0.0291(0)  0.0291(0)  0.0244(7)*  0.0244(3)
OTM Put 2    0.0173(1)  0.0170(3)  0.0165(6)*  0.0167(0)
OTM Put 3    0.0159(0)  0.0156(1)  0.0120(5)*  0.0121(4)
ATM Put 1    0.0263(0)  0.0263(0)  0.022(10)*  0.0222(0)
ATM Put 2    0.0153(4)  0.0153(1)  0.0155(4)*  0.0157(1)
ATM Put 3    0.0116(1)  0.0115(1)  0.0112(5)*  0.0114(3)
ATM Call 1   0.0263(0)  0.0264(0)  0.0227(9)*  0.0228(1)
ATM Call 2   0.0160(2)  0.0160(1)  0.0156(6)*  0.0157(1)
ATM Call 3   0.0120(0)  0.0120(1)  0.0112(7)*  0.0112(2)
OTM Call 1   0.0257(0)  0.0258(0)  0.0225(9)*  0.0225(1)
OTM Call 2   0.0144(1)  0.0143(5)* 0.0146(4)   0.0148(0)
OTM Call 3   0.0102(3)  0.0103(1)  0.0102(5)*  0.0103(1)
DOTM Call 1  0.0473(0)  0.0474(0)  0.0382(6)*  0.0382(4)
DOTM Call 2  0.0207(0)  0.0208(0)  0.0182(7)*  0.0183(3)
DOTM Call 3  0.0125(2)  0.0126(2)  0.0123(3)*  0.0123(3)

```


The table below shows the MCP at option groups level. The number of times that the model outperforms is shown in parenthesis (we have 10 stocks in total).
```python
                ecoDfm    ecoDfmX     fulDfm     fulDfmX
DOTM Put 1   0.5197(0)  0.5208(0)  0.5954(7)*   0.5949(3)
DOTM Put 2   0.4915(0)  0.4919(0)  0.5745(4)    0.5757(6)*
DOTM Put 3   0.5016(0)  0.5014(0)  0.5635(4)    0.5648(6)*
OTM Put 1    0.5473(0)  0.5475(0)  0.5876(3)    0.5883(7)*
OTM Put 2    0.5509(1)  0.5546(1)  0.5677(4)    0.5663(4)*
OTM Put 3    0.5351(0)  0.5376(0)  0.5738(0)    0.5786(10)*
ATM Put 1    0.5519(0)  0.5503(0)  0.5934(6)*   0.5931(4)
ATM Put 2    0.5480(2)  0.5430(0)  0.5651(6)*   0.5630(2)
ATM Put 3    0.5649(1)  0.5660(2)  0.5784(4)*   0.5778(3)
ATM Call 1   0.5445(0)  0.5430(0)  0.5881(5)    0.5891(5)*
ATM Call 2   0.5238(0)  0.5204(1)  0.5585(7)*   0.5559(2)
ATM Call 3   0.5272(0)  0.5212(0)  0.5747(9)*   0.5684(1)
OTM Call 1   0.5422(1)  0.5422(1)  0.5602(5)*   0.5592(3)
OTM Call 2   0.5190(0)  0.5199(1)  0.5343(5)*   0.5347(4)
OTM Call 3   0.5207(0)  0.5197(0)  0.5542(4)    0.5547(6)*
DOTM Call 1  0.5362(0)  0.5351(0)  0.5737(5)    0.5731(5)*
DOTM Call 2  0.5280(0)  0.5274(0)  0.5474(6)*   0.5480(4)
DOTM Call 3  0.5437(1)  0.5432(2)  0.5504(3)    0.5514(4)*

```