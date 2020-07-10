# |ᴄᴍʏᴋ| RMI Arrows
![PREVIEW](https://www.tradingview.com/x/UxWB7KLd/)
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
//@version=4
//                                                              STUDY
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
study("|ᴄᴍʏᴋ| RMI Arrows", overlay=true, precision=0)
//                                                              COLORS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
cyan        = #00CCCC
oker        = #FF9900
blues       = #0099CC
reds        = #FF6600
magenta     = #FF00FF
//                                                              INPUTS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
//                                                              RMI
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ 
SOURCE          = input(defval=close                        , title="Source")
ALPHA_PERIOD    = input(defval=30   , minval=1              , title="Alpha Period")
ALPHA_LOOPBACK  = input(defval=2    , minval=1              , title="Alpha Loopback")
ALPHA_AVERAGE   = input(defval=false                        , title="Alpha SMA Trendline")

BETA_PERIOD     = input(defval=300   , minval=1             , title="Beta Period")
BETA_LOOPBACK   = input(defval=20    , minval=1             , title="Beta Loopback")
BETA_AVERAGE    = input(defval=false                        , title="Beta SMA Trendline")

RANGE_NEAR      = input(defval=0.4   , minval=-1 , maxval=1 , title="Top Boundary")
RANGE_FAR       = input(defval=0.6   , minval=-1 , maxval=1 , title="High Boundary")

//                                                              FUNCTIONS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
//                                                              AVERAGING
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
//                      ROLLING MOVING AVERAGE
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
RMA( SERIES , LENGHT )=>
	ALPHA               = 1 / LENGHT
    float SUM           = na
    SUM                 := ALPHA * SERIES + ( 1.0 - ALPHA ) * nz( SUM[1] )
    
//                                                              INDEXING
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
//                      RELATIVE MOMENTUM INDEX
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
RMI( SOURCE , LENGHT , MOMENTUM )=> // USES LINEAR VALUES , MAKE WITH LOG VALUES LIKE CHART SCALING
    INC                 = RMA( max( SOURCE              - SOURCE[MOMENTUM]   , 0 )    , LENGHT)
    DEC                 = RMA( max( SOURCE[MOMENTUM]    - SOURCE             , 0 )    , LENGHT)
    RMI                 = DEC   == 0.0  ? 0.0     : (INC-DEC)/(INC+DEC)

//                                                              CALCULATIONS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
APLHA_RMI       = RMI(SOURCE,ALPHA_PERIOD,ALPHA_LOOPBACK)
BETA_RMI        = RMI(SOURCE,BETA_PERIOD,BETA_LOOPBACK)

//                                                              CONDITIONS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
AON = APLHA_RMI >= RANGE_NEAR
BON = BETA_RMI  >= RANGE_NEAR

AOF = APLHA_RMI >= RANGE_FAR
BOF = BETA_RMI  >= RANGE_FAR

AUN = APLHA_RMI <= -RANGE_NEAR
BUN = BETA_RMI  <= -RANGE_NEAR

AUF = APLHA_RMI <= -RANGE_FAR
BUF = BETA_RMI  <= -RANGE_FAR

//                                                              GRAPHICAL
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
plotshape(AUN, style=shape.triangleup, location=location.belowbar, color=cyan, size=size.small, transp=10)
plotshape(BUN, style=shape.triangleup, location=location.belowbar, color=blues, size=size.normal, transp=20)

plotshape(AUF, style=shape.triangleup, location=location.belowbar, color=cyan, size=size.small, transp=70)        
plotshape(BUF, style=shape.triangleup, location=location.belowbar, color=blues, size=size.normal, transp=90)

plotshape(AON, style=shape.triangledown, location=location.abovebar, color=oker, size=size.small, transp=10)
plotshape(BON, style=shape.triangledown, location=location.abovebar, color=reds, size=size.normal , transp=20)

plotshape(AOF, style=shape.triangledown, location=location.abovebar, color=oker, size=size.small, transp=70)
plotshape(BOF, style=shape.triangledown, location=location.abovebar, color=reds, size=size.normal , transp=90)
```
