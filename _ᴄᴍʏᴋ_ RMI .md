RMI 
![Preview](https://s3.tradingview.com/u/uXW9ej8i_mid.png)

## Introduction 
I started using this script because of its fast reaction, and good tell for buy/sell moments on a short timescale. 
For larger timescales, the overall trend should be taken into account regarding the levels. 

## Origin 
The Relative Momentum Index was developed by Roger Altman and was introduced in his article in the February, 1993 issue of Technical Analysis of Stocks & Commodities magazine. 
While RSI counts up and down days from close to close, the Relative Momentum Index counts up and down days from the close relative to a close x number of days ago. 
This results in an RSI that is smoother. 

## Adjustments 
CMYK color theme applied. 
Four levels to indicate intensity. 
Two Timescales, to overview the broader trend, and fast movements. 

## Usage 
RMI indicates `overbought` and `oversold` zones, and can be used for divergence and trend analysis. 

## Prospects 
* Self adjusting levels, relative to an SMA trend. 
* Alternative RMI, which functions as an overlay. 


```
//@version=2
study("CMYK RMI", scale=scale.right, overlay=false, precision=0)
// Relative Momentum Index
// Code is provided as public domain, no warranty
// Modified by MTVPMC
//
//****************************** COLORS *****************************
cyan        = #00CCCC
oker        = #FF9900
blues       = #0099CC
reds        = #FF6600
magenta     = #FF00FF

//****************************** INPUTS *****************************
FastLenght      = input(defval=30   , minval=1                  , type=integer  , title="Fast EMA Averaging Length")
FastMomentum    = input(defval=2    , minval=1                                  , title="Fast Lookback for Momentum")
FastInA         = input(defval=false                            , type=bool     , title="Fast SMA Trendline")

SlowLenght      = input(defval=250  , minval=1                  , type=integer  , title="Slow EMA Averaging Length")
SlowMomentum    = input(defval=20   , minval=1                                  , title="Slow Lookback for Momentum")
SlowInA         = input(defval=false                            , type=bool     , title="Slow SMA Trendline")

smaLen          = input(defval=10   , minval=1                  , type=integer  , title="SMA Period")

LTOP            = input(defval=30   , minval=00  , maxval=50    , type=integer  , title="Top Boundary")
LHIGH           = input(defval=20   , minval=00  , maxval=50    , type=integer  , title="High Boundary")

LLOW            = input(defval=-20  , minval=-50 , maxval=00    , type=integer  , title="Low Boundary")
LBOTTOM         = input(defval=-30  , minval=-50 , maxval=00    , type=integer  , title="Bottom Boundary")



//****************************** CALCULATION *****************************
FastemaInc      = ema(max(close - close[FastMomentum], 0), FastLenght)
FastemaDec      = ema(max(close[FastMomentum] - close, 0), FastLenght)
FastRMI         = FastemaDec == 0 ? 0 : 50 - 100 / (1 + FastemaInc / FastemaDec)

SlowemaInc      = ema(max(close - close[SlowMomentum], 0), SlowLenght)
SlowemaDec      = ema(max(close[SlowMomentum] - close, 0), SlowLenght)
SlowRMI         = SlowemaDec == 0 ? 0 : 50 - 100 / (1 + SlowemaInc / SlowemaDec)

//****************************** PLOT *****************************
PTOP            = plot(LTOP              , transp=20                            , title='Toplevel'                  , color=oker)
PHIGH           = plot(LHIGH             , transp=100                           , title='Highlevel'                 , color=oker)
PLOW            = plot(LLOW              , transp=100                           , title='Lowlevel'                  , color=cyan)
PBOTTOM         = plot(LBOTTOM           , transp=20                            , title='Bottomlevel'               , color=cyan)

FastoverTOP         = plot(FastRMI >= LTOP ? FastRMI : LTOP                     , title='FastOvertop'               , color=oker)
FastoverHIGH        = plot(FastRMI >= LHIGH ? FastRMI : LHIGH                   , title='FastOverHigh'              , color=oker)
FastunderLOW        = plot(FastRMI <= LLOW ? FastRMI : LLOW                     , title='FastUnderLow'              , color=cyan)
FastunderBOTTOM     = plot(FastRMI <= LBOTTOM ? FastRMI : LBOTTOM               , title='FastUnderbottom'           , color=cyan)

SlowoverTOP         = plot(SlowRMI >= LTOP ? SlowRMI : LTOP                     , title='SlowOvertop'               , color=oker)
SlowoverHIGH        = plot(SlowRMI >= LHIGH ? SlowRMI : LHIGH                   , title='SlowOverHigh'              , color=oker)

SlowunderLOW        = plot(SlowRMI <= LLOW ? SlowRMI : LLOW                     , title='SlowUnderLow'              , color=cyan)
SlowunderBOTTOM     = plot(SlowRMI <= LBOTTOM ? SlowRMI : LBOTTOM               , title='SlowUnderbottom'           , color=cyan)

plot(FastRMI                                                                    , title='FastRMI'                   , color=magenta)
plot(FastInA?sma(FastRMI,smaLen):na                                             , title='FastSMA'                   , color=cyan)

plot(SlowRMI                                                                    , title='SlowRMI'                   , color=white)
plot(SlowInA?sma(SlowRMI,smaLen):na                                             , title='SlowSMA'                   , color=blues)

//****************************** FILL *****************************
fill(FastoverTOP    , PTOP              , transp=20                             , title='FastTopfill'               , color=oker)
fill(FastoverHIGH   , PHIGH             , transp=90                             , title='FastHighfill'              , color=oker)

fill(PLOW       , FastunderLOW          , transp=90                             , title='FastLowfill'               , color=cyan)
fill(PBOTTOM    , FastunderBOTTOM       , transp=20                             , title='FastBottomfill'            , color=cyan)

fill(SlowoverTOP    , PTOP              , transp=20                             , title='SlowTopfill'               , color=reds)
fill(SlowoverHIGH   , PHIGH             , transp=90                             , title='SlowHighfill'              , color=reds)

fill(PLOW       , SlowunderLOW          , transp=90                             , title='SlowLowfill'               , color=blues)
fill(PBOTTOM    , SlowunderBOTTOM       , transp=20                             , title='SlowBottomfill'            , color=blues)

//****************************** LINES *****************************
hline(LTOP                                          , linestyle=dotted          , title='Toplevel'                  , color=reds)
hline(LHIGH                                         , linestyle=dotted          , title='Highlevel'                 , color=oker)

hline(00                                            , linestyle=dotted          , title='Midline'                   , color=gray)

hline(LLOW                                          , linestyle=dotted          , title='Lowlevel'                  , color=cyan)
hline(LBOTTOM                                       , linestyle=dotted          , title='Bottomlevel'               , color=blues)
```
