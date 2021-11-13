# TITLE
![PREVIEW](https://s3.tradingview.com/8/84VJskLO_mid.png)
[✖ Nunchucks](https://www.tradingview.com/script/84VJskLO-Nunchucks/)

A new way to display market data

* Does not flip color during current candle
* Displays Higher and Lower than the previous nunchuck's range
* Use it with current price line

![This is an image](https://s3.tradingview.com/snapshots/b/b3O79s7P.png)
![This is an image](https://s3.tradingview.com/snapshots/o/O3Z8cSqb.png)

## script
```
// This source code is subject to the terms of the Mozilla Public License 2_0 at https://mozilla_org/MPL/2_0/
// © MVPMC
//@version=4
study(title="✖ 𝗡𝘂𝗻𝗰𝗵𝘂𝗰𝗸𝘀", overlay=true)
bool Priceline  = input(title="Show Priceline" , defval=true)
bool Nullchucks = input(title="Show null chucks" , defval=true)
bool Chains     = input(title="Color chains as candlesticks" , defval=false)
string Theme    = input(title="Theme" , options=["TradingView","Grayscale","Japanese", "Blush" , "Matrix" , "Nightdrive", "Colorblind", "idunno", "CMYK"], defval="TradingView")

High_Color      = Theme == "TradingView" ? #26a69a : Theme == "Grayscale" ? #808080 : Theme == "Japanese" ? #FFFFFF : Theme == "CMYK" ? #03e8d6 : Theme == "Blush" ? #e7a0a9 : Theme == "Matrix" ? #63cb94 : Theme == "Nightdrive" ? #4fa7e5 : Theme == "Colorblind" ? #00a2cb : Theme == "idunno" ? #249ba9 : #FF00FF
Low_Color       = Theme == "TradingView" ? #ef5350 : Theme == "Grayscale" ? #404040 : Theme == "Japanese" ? #000000 : Theme == "CMYK" ? #e304eb : Theme == "Blush" ? #503e44 : Theme == "Matrix" ? #34734c : Theme == "Nightdrive" ? #9b68ae : Theme == "Colorblind" ? #ffb400 : Theme == "idunno" ? #e13268 : #FF00FF
Center_Color    = Theme == "TradingView" ? #808080 : Theme == "Grayscale" ? #606060 : Theme == "Japanese" ? #808080 : Theme == "CMYK" ? #f0af03 : Theme == "Blush" ? #c0999e : Theme == "Matrix" ? #141e19 : Theme == "Nightdrive" ? #a5cfea : Theme == "Colorblind" ? #858585 : Theme == "idunno" ? #765b86 : #FF00FF


High_Display    = high > high[1] ? true : false
High_Open       = min(high, high[1])
High_Close      = high

Low_Display     = low < low[1] ? true : false
Low_Open        = max(low, low[1])
Low_Close       = low

Center_open     = Low_Open < High_Open ? Low_Open : High_Open
Center_close    = Low_Open > High_Open ? High_Open : Low_Open 

Wickcolor       = Chains ? open < close ? High_Color : Low_Color : Center_Color
Pricecolor      = Priceline ? close > High_Open ? High_Color : close < Low_Open ? Low_Color : Center_Color : #00000000

plotHighcolor   = Nullchucks ? High_Color : High_Display ? High_Color : #00000000
plotLowcolor    = Nullchucks ? Low_Color : Low_Display ? Low_Color : #00000000

plotcandle(High_Open,   High_Close,     Center_open,    High_Close, title="Higher", color=plotHighcolor,    bordercolor=Theme == "Japanese" ? #000000 : plotHighcolor,  wickcolor=Wickcolor)
plotcandle(Low_Open,    Center_close,   Low_Close,      Low_Close,  title="Lower" , color=plotLowcolor,     bordercolor=plotLowcolor,   wickcolor=Wickcolor)

plot(close, title="Priceline", color=Pricecolor,    trackprice=Priceline,   show_last=1)
```
An attempt at Signals
```
//@version=4
study("✖ 𝗡𝘂𝗻𝗰𝗵𝘂𝗰𝗸 𝗣𝗮𝘁𝘁𝗲𝗿𝗻𝘀", overlay=true)
Nunchuck = high > high[1] ? low < low[1] ? true : false : false
Highchuck = high > high[1] ? low > low[1] ? true : false : false
Lowchuck = high < high[1] ? low < low[1] ? true : false : false
Nullchuck = high < high[1] ? low > low[1] ? true : false : false

plotshape(Nunchuck,style=shape.diamond,location=location.abovebar, color=#FFFFFF)
plotshape(Nunchuck,style=shape.diamond,location=location.belowbar, color=#FFFFFF)
plotshape(Highchuck,style=shape.triangleup,location=location.abovebar, color=#80FF80)
plotshape(Lowchuck,style=shape.triangledown,location=location.belowbar, color=#FF8080)
plotshape(Nullchuck,style=shape.xcross,location=location.abovebar, color=#8080FF)
plotshape(Nullchuck,style=shape.xcross,location=location.belowbar, color=#8080FF)

//Nunchucks
Bullchuck = Lowchuck[1] and Nunchuck
plotshape(Bullchuck,style=shape.triangleup,location=location.belowbar, color=#80FFFF, size=size.normal)
Bearchuck = Highchuck[1] and Nunchuck
plotshape(Bearchuck,style=shape.triangledown,location=location.abovebar, color=#FF80FF, size=size.normal)

//Nullchucks
BullNull = Highchuck[2] and Nunchuck[1] and Nullchuck
plotshape(BullNull,style=shape.triangleup,location=location.belowbar, color=#208080, size=size.tiny)


//Pennants
Bullpennant4 = Highchuck[3] and Highchuck[2] and Nullchuck[1] and Nullchuck
plotshape(Bullpennant4,style=shape.triangleup,location=location.belowbar, color=#80FF80, size=size.normal)
Bullpennant3 = Highchuck[2] and Nullchuck[1] and Nullchuck and not Bullpennant4
plotshape(Bullpennant3,style=shape.triangleup,location=location.belowbar, color=#80FF80, size=size.small)
Bullpennant2 = Highchuck[1] and Nullchuck 
plotshape(Bullpennant2,style=shape.triangleup,location=location.belowbar, color=#80FF80, size=size.tiny)

Bearpennant4 = Lowchuck[3] and Lowchuck[2] and Nullchuck[1] and Nullchuck
plotshape(Bearpennant4,style=shape.triangledown,location=location.abovebar, color=#FF8080, size=size.normal)
Bearpennant3 = Lowchuck[2] and Nullchuck[1] and Nullchuck and not Bearpennant4
plotshape(Bearpennant3,style=shape.triangledown,location=location.abovebar, color=#FF8080, size=size.small)
Bearpennant2 = Lowchuck[1] and Nullchuck
plotshape(Bearpennant2,style=shape.triangledown,location=location.abovebar, color=#FF8080, size=size.tiny)
```
