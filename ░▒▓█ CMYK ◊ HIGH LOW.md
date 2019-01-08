# ░▒▓█ CMYK ◊ HIGH LOW
![PREVIEW](https://s3.tradingview.com/9/9bWAlzC4_mid.png)
## Introduction 
This script is intended to display High and Low values, alongside Highest and Lowest of an adjustable period 

## Adjustments 
CMYK color theme applied. 
Price color changes if EMA >< EMA 

##  Usage 
For those who use Line, Break, or Area Type Charts. 
Usefull to quickly display proper levels for buying selling on minute-term trading 

##  Future Prospects 
*

```
//@version=3
study("CMYK HIGH LOW", overlay=true, scale=scale.right)
// HIGH LOW
// Code is provided as public domain, no warranty
// Created by MTVPMC
//
//****************************** COLORS ******************************
cyan        = #00CCCC
oker        = #FF9900
blues       = #0099CC
reds        = #FF6600
magenta     = #FF00FF

//****************************** INPUTS *****************************
range       = input(defval=5   , minval=1              , type=integer      , title="Lag")

//****************************** PLOT *****************************
plot(ema(low,1) , color=cyan, transp=00, trackprice=true, title="Low")
plot(ema(high,1) , color=oker, transp=00, trackprice=true, title="High")

	

plot(lowest(low,range), color=blues, transp=40, trackprice=true, title="Lowest")
plot(highest(high,range) , color=reds, transp=40, trackprice=true, title="Highest")

//****************************** Barcolor *****************************
mysignal = ema(close, range) - ema(close, range * 2)
//barcolor(mysignal[0] > mysignal[1] ? cyan : oker)
```
