---
title: "Option contracts availability "
date: 2018-09-12T14:51:12+06:00
author: Michael
description: Uses Async call to lucene index for super fast autocompletion to address performance issue loading config.
featured: true
categories: ["cat1"]
tags: ["java","jQuery"]
---


# Option contracts availability 

## 1 Contracts scatter plot 
The following figure plots the option contracts against its delta and maturity. This figure just simply gives a feeling how 
options contracts distributed along delta and maturity dimensions. Quite surprising, there is a clear cutoff at 240 days
maturity (except for AAPL). Based on this observation, I add a number in each subplot title which represents the percentage
of contracts that with maturity less than 240 days. It shows that at least 70% of contracts are with maturity less than 240 
days. Note that this figure only plots **Out-of-money** options.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/contrscatter.png)

Based on this observation, I think there is no need to go beyond 240 days for equity options.

Then I zoom out the first 240 maturity days, it seems that conditional on the first 240 maturity days, 
almost half of contracts lie within 60 days.
![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/contrscatter60.png)


## 2 Contracts availability distribution 
The following figure plots the option contracts availability. Specifically, I first divide the maturity in to 20 days interval,
then check whether there is available contracts within each maturity interval on daily basis. Then I count how many days we have 
available contracts in that interval, finally I divide the number of days with available contracts by the sample length, which
is 3,438 days in our case. Then this figure plot the percentage of days we have available contracts (percentage of 3,438 days).

The x axis ticks should be interpreted as, for example, 40 means within maturity interval [20,40], 60 means within maturity 
interval [40,60], and etc. The interpretation of figure should be, for example, for ATM Put option of stock AAPL at maturity 
interval [40,60], about 58% of time (1994 (3438*0.58) out of 3438 days) we have available contracts, while the remaining 
(3438-1994=1444) dates we do not have available contracts. 

![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/20distr.png)

As seen, once the maturity goes beyond 60 days, the availability plummets to about 20%-30%. However,it might become better 
once we  choose wider maturity interval **if** the contracts available at different dates. Next, I check the option availability
by expand the maturity interval to 80 days. The x axis ticks should be interpreted as that, for example, 160 means maturity
interval [80,160].

![](https://dl.dropboxusercontent.com/u/97697694/ImvolSurf/Update20161206/80distr.png)

* In general, setting the maturity group as [0,80], [80,160], and [160,240] is much more appropriate.