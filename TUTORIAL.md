Getting Started Writing fxLabs Tools with MQL4
==============================================

This document provides a tutorial for writing your own fxLabs tools with MQL4. Familiarity with MQL4 is assumed. (If you are not famliar with MQL4, see [book.mql4.com](http://book.mql4.com/) or [docs.mql4.com](http://docs.mql4.com/) for additional instructional material.) 

Before continuing, ensure that you have the fxlabs-MQL4 package installed on your machine. Details for doing this can be found [here](https://github.com/oanda/mt4-fxlabs).

## First steps

* Insert the directive `#include <fxlabsnet.mqh>` at the beginning of your MQL4 source. 

* In your `init()` or `OnInit()` method, add a call to `init_fxlabs()`. This needs to be done before any other calls to methods in `fxlabsnet.mqh` are made. 

## General Approach to Retrieving fxLabs Data

There are five different types of fxLabs data you can retrieve using fxLabs MQL4 API:

1. Calendar
2. Historical Position Ratios (HPR)
3. Orderbook
4. Spreads
5. Commitments of Traders

`fxlabsnet.mqh` is broken down into sections, each of which details methods for retrieving one of the above types of data. 

The basic approach for retrieving any of these types of data is to make a call to retrieve the data, specifying the amount of historical data wanted (this value is specified in seconds and is called the 'period'). This will return a reference id to the retrieved data, which can then be used in subsequent calls to extract specific parts of the retrieved data. 

This tutorial will cover retrieving Historical Position Ratios, but the same ideas apply to the other types of data. 


