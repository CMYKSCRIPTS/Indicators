![PREVIEW](https://s3.tradingview.com/w/WqlNSIjd_mid.png)
# ░▒▓█ CMYK ◊ RMI ARROWS
## Introduction 
I started using this script because of its fast reaction, and good tell for buy/sell moments. 
For larger timescales, the overall trend should be taken into account regarding the levels. 
In the future i will update this indicator, to automatically adjust those. 

This is the Overlay expansion of the Indicator Linked below. 

## Origin 
The Relative Momentum Index was developed by Roger Altman and was introduced in his article in the February, 1993 issue of Technical Analysis of Stocks & Commodities magazine. 
While RSI counts up and down days from close to close, the Relative Momentum Index counts up and down days from the close relative to a close x number of days ago. 
This results in an RSI that is smoother. 

## Adjustments 
CMYK color theme applied. 
Four levels to indicate intensity. 
Two Timescales, to overview the broader trend, and fast movements. 

## Usage 
RMI indicates overbought and oversold zones, and can be used for divergence and trend analysis. 

## Future Prospects 
* Self adjusting levels, relative to an SMA trend. 
* Alternative RMI, which functions as an overlay. 

```
//@version=3
study("CMYK RMI ARROWS", scale=scale.right, overlay=true, precision=0)
// Relative Momentum Index
// Code is provided as public domain, no warranty
// Modified by MTVPMC

//****************************** COLORS ****************************
cyan        = #00CCCC
oker        = #FF9900
blues       = #0099CC
reds        = #FF6600
magenta     = #FF00FF

//****************************** INPUTS *****************************
FastLenght      = input(defval=30   , minval=1              , type=integer      , title="Fast EMA Averaging Length")
FastMomentum    = input(defval=2    , minval=1                                  , title="Fast Lookback for Momentum")
FastInA         = input(defval=false                        , type=bool         , title="Fast SMA Trendline")

SlowLenght      = input(defval=250  , minval=1              , type=integer      , title="Slow EMA Averaging Length")
SlowMomentum    = input(defval=20   , minval=1                                  , title="Slow Lookback for Momentum")
SlowInA         = input(defval=false                        , type=bool         , title="Slow SMA Trendline")

smaLen          = input(defval=10   , minval=1              , type=integer      , title="SMA Period")

LTOP            = input(defval=30   , minval=00 , maxval=50, type=integer       , title="Top Boundary")
LHIGH           = input(defval=20   , minval=00 , maxval=50, type=integer       , title="High Boundary")

LLOW            = input(defval=-20   , minval=-50 , maxval=00 , type=integer    , title="Low Boundary")
LBOTTOM         = input(defval=-30   , minval=-50 , maxval=00 , type=integer    , title="Bottom Boundary")

//****************************** CALCULATION *****************************
FastemaInc      = ema(max(close - close[FastMomentum], 0), FastLenght)
FastemaDec      = ema(max(close[FastMomentum] - close, 0), FastLenght)
FastRMI         = FastemaDec == 0 ? 0 : 50 - 100 / (1 + FastemaInc / FastemaDec)

SlowemaInc      = ema(max(close - close[SlowMomentum], 0), SlowLenght)
SlowemaDec      = ema(max(close[SlowMomentum] - close, 0), SlowLenght)
SlowRMI         = SlowemaDec == 0 ? 0 : 50 - 100 / (1 + SlowemaInc / SlowemaDec)

SADT = SlowRMI >= LTOP
FADT = FastRMI >= LTOP

SADH = SlowRMI >= LHIGH
FADH = FastRMI >= LHIGH

SAUL = SlowRMI <= LLOW
FAUL = FastRMI <= LLOW

SAUB = SlowRMI <= LBOTTOM
FAUB = FastRMI <= LBOTTOM

//****************************** PLOT *****************************
plotshape(FADT, style=shape.triangledown, location=location.abovebar, color=oker, size=size.small, transp=10)
plotshape(SADT, style=shape.triangledown, location=location.abovebar, color=reds, size=size.normal , transp=20)

plotshape(FADH, style=shape.triangledown, location=location.abovebar, color=oker, size=size.small, transp=70)
plotshape(SADH, style=shape.triangledown, location=location.abovebar, color=reds, size=size.normal , transp=90)

plotshape(FAUL, style=shape.triangleup, location=location.belowbar, color=cyan, size=size.small, transp=70)        
plotshape(SAUL, style=shape.triangleup, location=location.belowbar, color=blues, size=size.normal, transp=90)

plotshape(FAUB, style=shape.triangleup, location=location.belowbar, color=cyan, size=size.small, transp=10)
plotshape(SAUB, style=shape.triangleup, location=location.belowbar, color=blues, size=size.normal, transp=20)
```
