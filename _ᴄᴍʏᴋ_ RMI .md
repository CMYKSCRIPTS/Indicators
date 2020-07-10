# |ᴄᴍʏᴋ| RMI 
![Preview](https://www.tradingview.com/x/a9RgmFiX/)

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
//@version=4
//                                                              STUDY
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
study("|ᴄᴍʏᴋ| RMI", overlay=false, precision=0)
//                                                              COLORS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
white           = #FFFFFF
gray            = #888888
cyan            = #00CCCC
oker            = #FF9900
blues           = #0099CC
reds            = #FF6600
magenta         = #FF00FF
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

RANGE_NEAR      = input(defval=0.4   , minval=-1 , maxval=1 , title="Near Boundary")
RANGE_FAR       = input(defval=0.6   , minval=-1 , maxval=1 , title="Far Boundary")

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
ALPHA_RMI       = RMI(SOURCE,ALPHA_PERIOD,ALPHA_LOOPBACK)
BETA_RMI        = RMI(SOURCE,BETA_PERIOD,BETA_LOOPBACK)

//                                                              CONDITIONS
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
AON = ALPHA_RMI >= RANGE_NEAR
BON = BETA_RMI  >= RANGE_NEAR

AOF = ALPHA_RMI >= RANGE_FAR
BOF = BETA_RMI  >= RANGE_FAR

AUN = ALPHA_RMI <= -RANGE_NEAR
BUN = BETA_RMI  <= -RANGE_NEAR

AUF = ALPHA_RMI <= -RANGE_FAR
BUF = BETA_RMI  <= -RANGE_FAR

//                                                              GRAPHICAL
//░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
//                                                              PLOT
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
PTOP                = plot(RANGE_FAR         , transp=20                , title="Toplevel"                  , color=oker)
PHIGH               = plot(RANGE_NEAR        , transp=100               , title="Highlevel"                 , color=oker)
PLOW                = plot(-RANGE_NEAR       , transp=100               , title="Lowlevel"                  , color=cyan)
PBOTTOM             = plot(-RANGE_FAR        , transp=20                , title="Bottomlevel"               , color=cyan)

AlphaoverHIGH       = plot(AON ? ALPHA_RMI  : RANGE_NEAR                , title="AlphaOverHigh"             , color=oker)
BetaoverHIGH        = plot(BON ? BETA_RMI   : RANGE_NEAR                , title="BetaOverHigh"              , color=oker)

AlphaoverTOP        = plot(AOF ? ALPHA_RMI  : RANGE_FAR                 , title="AlphaOvertop"              , color=oker)
BetaoverTOP         = plot(BOF ? BETA_RMI   : RANGE_FAR                 , title="BetaOvertop"               , color=oker)

AlphaunderLOW       = plot(AUN ? ALPHA_RMI  : -RANGE_NEAR               , title="AlphaUnderLow"             , color=cyan)
BetaunderLOW        = plot(BUN ? BETA_RMI   : -RANGE_NEAR               , title="BetaUnderLow"              , color=cyan)

AlphaunderBOTTOM    = plot(AUF ? ALPHA_RMI  : -RANGE_FAR                , title="AlphaUnderbottom"          , color=cyan)
BetaunderBOTTOM     = plot(BUF ? BETA_RMI   : -RANGE_FAR                , title="BetaUnderbottom"           , color=cyan)

plot(ALPHA_RMI                                                          , title="ALPHA_RMI"                 , color=magenta)
plot(ALPHA_AVERAGE?sma(ALPHA_RMI,ALPHA_PERIOD):na                       , title="AlphaSMA"                  , color=cyan)

plot(BETA_RMI                                                           , title="BETA_RMI"                  , color=white)
plot(BETA_AVERAGE?sma(BETA_RMI,BETA_PERIOD):na                          , title="BetaSMA"                   , color=blues)
//                                                              FILL
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
fill(AlphaoverTOP    , PTOP                 , transp=20                 , title="AlphaTopfill"              , color=oker)
fill(AlphaoverHIGH   , PHIGH                , transp=90                 , title="AlphaHighfill"             , color=oker)

fill(PLOW           , AlphaunderLOW         , transp=90                 , title="AlphaLowfill"              , color=cyan)
fill(PBOTTOM        , AlphaunderBOTTOM      , transp=20                 , title="AlphaBottomfill"           , color=cyan)

fill(BetaoverTOP    , PTOP                  , transp=20                 , title="BetaTopfill"               , color=reds)
fill(BetaoverHIGH   , PHIGH                 , transp=90                 , title="BetaHighfill"              , color=reds)

fill(PLOW           , BetaunderLOW          , transp=90                 , title="BetaLowfill"               , color=blues)
fill(PBOTTOM        , BetaunderBOTTOM       , transp=20                 , title="BetaBottomfill"            , color=blues)
//                                                              LINES
//▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
hline(RANGE_FAR     , linestyle=hline.style_dotted                      , title="Toplevel"                  , color=reds)
hline(RANGE_NEAR    , linestyle=hline.style_dotted                      , title="Highlevel"                 , color=oker)

hline(00            , linestyle=hline.style_dotted                      , title="Midline"                   , color=gray)

hline(- RANGE_NEAR  , linestyle=hline.style_dotted                      , title="Lowlevel"                  , color=cyan)
hline(- RANGE_FAR   , linestyle=hline.style_dotted                      , title="Bottomlevel"               , color=blues)
```
