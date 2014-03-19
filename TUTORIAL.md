Tutorial: Using the fxLabs MQL4 API
===================================

This document provides a tutorial for writing your own fxLabs tools with MQL4. Familiarity with MQL4 is assumed. If you are not familiar with MQL4, please refer to [book.mql4.com](http://book.mql4.com/) and [docs.mql4.com](http://docs.mql4.com/) for additional instructional material. 

Before continuing, ensure that you have the fxlabs-MQL4 package installed on your machine. Details for doing this can be found [here](https://github.com/oanda/mt4-fxlabs).

### First steps

* Insert the directive `#include <fxlabsnet.mqh>` at the beginning of your MQL4 source. 

* In your `init()` or `OnInit()` method, add a call to `init_fxlabs()`. This needs to be done before any other calls to methods in `fxlabsnet.mqh` are made. 

### Retrieving fxLabs Data

There are five different types of fxLabs data you can retrieve using the fxLabs MQL4 API:

1. Calendar
2. Historical Position Ratios (HPR)
3. Orderbook
4. Spreads
5. Commitments of Traders

`fxlabsnet.mqh` is broken down into sections, each of which details methods for retrieving one of the above types of data.

The basic approach for retrieving any of these types of data is to first make a call that specifies the `instrument` and `period` of time for which data is desired. This will return a reference id to the retrieved data, which can then be used in subsequent calls to extract specific parts of the retrieved data. 

Below we describe how to retrieve data for Historical Position Ratios, but the basic approach applies to all types of fxLabs data. 

### Retrieving HPR data

To retrieve the last month of HPR data for EUR/USD, use the following code segment:

```
int ref = hpr("EUR/USD",60*60*24*30); 
```

The first and second arguments specify the instrument and period of time in seconds for which data is being retrieved, respectively. The return value is a reference id to the data retrieved, which is used in separate fxlabs API calls to extract specific parts of the retrieved data. 

The instrument passed into `hpr(...)` as the first argument must be formatted with a forward slash between the base and quote currencies. Since MT4 symbols do not have this formatting by default, you need to insert it yourself. This can be easily done as follows:
 
```
string symbol = Symbol(); 
symbol = StringConcatenate(StringSubstr(symbol, 0, 3), "/", StringSubstr(symbol, 3));

int ref = hpr(symbol,60*60*24*30); 
```

`Symbol()`, `StringConcatenate()`, and `StringSubstr()` are all standard MQL4 methods. 

### How much HPR data was retrieved?

The preceding call to `hpr(...)` gives you a reference id, `ref`, to the retrieved data, but it does not return the actual data itself. To extract the data, first you need to know how many data points that you are dealing with. 

This is obtained by calling `hpr_sz`:

```
int sz = hpr_sz(ref); 
```

### Extracting HPR Data Piece-By-Piece

Each HPR data point has two pieces of information: a `timestamp`, and a `percentage`. The `percentage` value is the percentage of market orders that were long at the given `timestamp`. The percentage of market orders that were short at the same `timestamp` is just `100.0 - percentage`. 

To get the timestamp of the `i`th data point, where `0<=i<sz`, make the following call:

```
int timestamp = hpr_ts(ref, i); 
```

To get the percentage of long market orders at the `i`th data point, where `0<=i<=sz`, make the following call:

```
double percentage = hpr_percentage(ref, i);
```

### Extract HPR Data Into Parallel Arrays

As an alternative to extracting HPR data piece-by-piece, as described above, a convenience method is provided for extracting all the retrieved data into parallel arrays. 

First, create two dynamic arrays and resize them to the number of data points being extracted:

```
int ts[]; 
double perc[]; 

ArrayResize(ts, sz); 
ArrayResize(perc, sz); 
```

(`ArrayResize` is a standard MQL4 method.) 

Once this is done, call the `hpr_data(...)` method to fill in the arrays:

```
bool success = hpr_data(ref, ts, perc); 
```

The first argument is the reference id returned by the call to `hpr(...)`, and the second and third arguments are the arrays to be filled.  The method will return `true` on success, and `false` otherwise. The arrays are filled in "parallel" so that `ts[i]` corresponds to the timestamp associated with the percentage of long market orders in `perc[i]`. 

### A Note about Timestamps

Timestamps returned by the fxLabs API correspond to UNIX timestamps. Due to a bug in the way MT4 treats timestamps, if you want to use the timestamps returned by the fxLabs API in an MT4 chart, you first need to shift the value of the timestamp to be consistent with MT4s broken timestamps. A convenience method, `convert_to_mt4_time`, exists for doing this:

```
int timestamp = hpr_ts(ref, 0);   // retrieve 0th timestamp in data
int mt4_time = convert_to_mt4_time(timestamp); 
```

Alternatively, if you have an array of timestamps, `ts`, you can use `convert_to_mt4_time_arr`:

```
int ts[]; 
int perc[]; 

ArrayResize(ts, sz); 
ArrayResize(perc, sz); 

bool success = hpr_data(ref, ts, perc); 

if (success) {
   convert_to_mt4_time_arr(ts); 
}
```

The method `convert_to_mt4_time_arr` modifies the array passed as an argument and does not return anything. 

Please note that the timestamps returned by the fxLabs MQL4 API are in fact the correct time stamps. Conversion is only necessary if you intend to use timestamps to draw points within MT4 charts. 

### Next Steps

In this tutorial we have described the basics of retrieving historical position ratios from the fxLabs API. Retrieving other types of data using the fxLabs MQL4 API follows a similar approach. 

Detailed documentation for the fxLabs MQL4 API methods is provided in the header file `fxlabsnet.mqh`. There are also several example scripts and indicators, with full source, provided in the [fxLabs-MQL4 package](https://github.com/oanda/mt4-fxlabs). 
