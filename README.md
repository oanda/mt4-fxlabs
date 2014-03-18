MT4-fxLabs
==========

The MT4-fxLabs project brings the power of [OANDA Forex Labs](http://fxtrade.oanda.com/analysis/labs/) applications to [MT4](http://fxtrade.oanda.com/trade-forex/metatrader/). 

You can either use one of several pre-existing custom indicators/scripts in your MT4 client, or you can create your own using the MQL4 API for fxLabs. 

# Installation of MT4-fxLabs Tools

## Prerequisites 

* MT4 Client, build 600 or greater. This can be downloaded by following the instructions at OANDA's [MT4 Account Setup](http://fxtrade.oanda.com/trade-forex/metatrader/trade-account-setup) page. 

* .NET3.5 framework. 


## Instructions

1. Download fxlabs-MQL4.zip

2. In your MT4 Client, click File->Open Data Folder. Copy fxlabs-MQL4.zip to this folder, and unzip. The contents should unzip into the appropriate subdirectories in this folder. 

3. Using Windows File Explorer, go to the MQL4\Libraries subdirectory, right-click on fxlabsnet.dll, and choose 'Properties'. Click the 'Unblock' button if it's present. (If the 'Unblock' button is not there, then don't worry about this step.) 

4. Restart your MT4 client if it is currently running. 

You should now see new entries under the 'Custom Indicators' and 'Scripts' sections of the Navigator panel. The available indicators and scripts are described below. 

# Available Custom Indicators/Scripts

## Indicator: OANDA_Historical_Position_Ratios

Graph OANDA's historical ratios between long and short positions held by customers. 

## Indicator: OANDA_Spreads

Graph OANDA's historical spread values (ask-bid difference) for an instrument. 

## Indicator: OANDA_Commitments_of_Traders

Graph Commitments of Traders data from the [CFTC](http://www.cftc.gov/MarketReports/CommitmentsofTraders/index.htm). 

## Indicator: OANDA_Orderbook

Graph OANDA's Orderbook data. See [OANDA Forex Order Book](http://fxtrade.oanda.ca/analysis/forex-order-book) for more details. 

MT4 is limited to how it can show the orderbook data in a price vs time chart window, so the indicator does not display the same as what's on the fxLabs page. It is, however,  based on the same data. 

In particular, the MT4 indicator graphs two bars, similar to the Historical Position Ratios indicator. The top one is the percentage of long orders with trigger prices above the high price of the candle, and the lower bar is the percentage of short orders with trigger prices below the low price of the candle. This is intended to give an idea of how strong market sentiment is that the instrument's price will increase or decrease. 

## Script: fxlabs_calendar_demo

Draws markers associated with calendar headlines overlaid on a chart window.

## Script: fxlabs_hpr_demo

Similar to the Historical Position Ratio indicator, except it will draw the HPR graph overlaid
on the chart window, instead of drawing the graph in a separate indicator window. 

# How To Write Your Own MQL4 based fxLabs tools

To write your own fxLabs tools in MT4, add the declaration `#include <fxlabsnet.mqh>` to the beginning of your MQL4 source. The various methods that you can call are documented inline in the fxlabsnet.mqh file. A [tutorial](https://github.com/oanda/mt4-fxlabs/blob/master/TUTORIAL.md) to help you get started is also available. 
