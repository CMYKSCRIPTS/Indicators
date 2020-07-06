# |ᴄᴍʏᴋ| High Low
![PREVIEW](https://www.tradingview.com/x/m7HfTyYI/)
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
//@version=4
//                                                                              STUDY
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
study(title='|ᴄᴍʏᴋ| High Low', shorttitle='|ᴄᴍʏᴋ| HL', overlay=true)
//                                                                              COLORSPACE
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
cyan        = #00CCCC
oker        = #FF9900
blues       = #0099CC
reds        = #FF6600
magenta     = #FF00FF
//                                                                              INPUTS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
PERIOD      = input(defval=10   , minval=1     , title="Period")
//                                                                              GRAPHICAL
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
//                                                              LOW HIGH
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
plot(ema(high,1)                , color=oker, transp=00, trackprice=true, title="High")
plot(ema(low,1)                 , color=cyan, transp=00, trackprice=true, title="Low")
//                                                              LOWEST HIGHGEST
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
plot(highest(high   ,PERIOD)    ,color=reds , transp=40, trackprice=true, title="Highest")
plot(lowest(low     ,PERIOD)    ,color=blues, transp=40, trackprice=true, title="Lowest")
```
