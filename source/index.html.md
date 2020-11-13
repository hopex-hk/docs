---
title: Hopex API 

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='https://web.hopex.com/myaccount?page=ApiManage&lang=en'>Create API Key</a>

includes:

search: true

code_clipboard: false
---    

# Introduction    

## API Introduction

Welcome to Hopex API！

This is the official Hopex API document, and will be continue updating, please follow us to get latest news.

You can switch to different language by clicking the button in the top right.

The example of request and response is showing in the right hand side.

## Request Process   

Root URL for REST access：`https://api2.hopex.com/api/v1/` 

All requests are based on Https protocol
	
Request Process Description

1. Request parameters: parameter encapsulation according to interface request parameter specifications.
2. Submit request parameters: submit the constructed request parameters to the server through POST or GET method.
3. Server response: the server first performs a parameter security check on the user's request data, and after authentication check, the response data is returned to the user in JSON format according to business logic.
4. Data processing: process the server response. 

Special note:
Some apis are response in Chinese default, if you prefer English version, please add the parameter: `culture=en` at every request.

# API Access

## Overview

|Request Method| Request Url | Description|
| :-----    | :-----   | :-----    |
|Get| [/api/v1/ticker](#get-hopex-contract-data) | Get Hopex Contract Data |
|Post| [/api/v1/depth](#get-hopex-market-depth-information) | Get Hopex Market Depth Information |
|Get| [/api/v1/trades](#get-hopex-latest-completed-orders) | Get Hopex Latest Completed Orders |
|Get| [/api/v1/kline](#get-hopex-kline-data) | Get Hopex Kline Data |
|Get| [/api/v1/markets](#get-hopex-markets-data) | Get Hopex Markets Data |
|Get| [/api/v1/indexStat](#get-hopex-volume) | Get Hopex Volume |
|Get| [/api/v1/index_notify](#get-hopex-notifies) | Get Hopex Notifies |
|Get| [/api/v1/userinfo](#get-hopex-user-information) | Get Hopex User Information |
|Post| [/api/v1/order](#place-orders) | Place Orders |
|Get| [/api/v1/cancel_order](#cancel-orders) | Cancel Orders |
|Get| [/api/v1/order_info](#get-active-orders-information) | Get Active Orders information |
|Post| [/api/v1/order_history](#get-history-orders) | Get History Orders |
|Get| [/api/v1/position](#get-positions-information) | Get Positions Information |
|Post| [/api/v1/condition_order](#place-condition-orders) | Place Condition Orders |
|Post| [/api/v1/condition_order_info](#get-condition-orders-infomation) | Get Condition Orders Infomation |
|Post| [/api/v1/cancel_condition_order](#cancel-condition-orders) | Cancel Condition Orders |
|Get| [/api/v1/wallet](#get-asset-information) | Get Asset Information |
|Get| [/api/v1/set_leverage](#set-contract-leverage) | Set Contract Leverage |
|Post| [/api/v1/get_orderParas](#get-order-parameter) | Get Order Parameter |
|Post| [/api/v1/liquidation_history](#get-liquidation-history) | Get Liquidation History |
|Get| [/api/v1/account_records](#get-deposit-and-withdraw-history) | Get Deposit And Withdraw History |

## Request Format

The API is restful and there are two method: GET and POST.

* GET request：All parameters are included in URL
* POST request: : All parameters are formatted as JSON and put int the request body
* You should Add `Authorization`,`Date`,`Digest` to request header when calling API that require API Key,You can Refer to [Authentication Details](#api-authentication)

## Response Format

The response is JSON format.

```json
{
  "ret": 0,
  "errCode": null,
  "errStr": null,
  "timestamp": 1604975001875,
  "data": // per API response data in nested JSON object
}
```

|Field| Data Type | Description|
| :-----    | :-----   | :-----    |
|ret       | int    | Status of API response, 0:success, -1:error|
|errCode   | string    | Error code of API response|
|errStr    | string   | Error Message of API response|
|data      | object    | The body data in response|
|timestamp | long    | The UTC timestamp when API respond, the unit is millisecond |

## Data Type

The JSON data type described in this document is defined as below:

- `string`: a sequence of characters that are quoted
- `int`: a 32-bit integer, mainly used for status code, size and count
- `long`: a 64-bit integer, mainly used for Id and timestamp
- `float`: a fraction represented in decimal format, mainly used for volume and price, recommend to use high precision decimal data types in program
- `bool`: false and true

# Frequently Asked Questions

## Access And Authentication

### Q1: How many API Keys one user can apply?
A: Every user can create 1 read API Key and 1 trade API Key

### Q2: What's difference between trade API Key and read API Key?
A: Read API Key Can't access API `order`、`cancel_order`、`condition_order`、`cancel_condition_order`,trade API Key can access all APIs

### Q3: Access API return "HMAC signature does not match"
A: Please check below possible reasons:

1. Use API Key or API Secret Incorrect

2. The request line in request header authorization may not the same as request API

### Q4: Access API return "API rate limit exceeded"
A: Access API More Frequently

## Market Data

### Q1: What is the update frequency of market depth?
A: The data is updated in real-time

### Q2: Could the total volume of Last 24h Market Summary decrease?
A: Yes, it is possible that the accumulated volume and the accumulated value counted for current 24h window is smaller than previous.

### Q3: How to retrieve the last trade price in market?
A: It is suggested to request to `GET /api/v1/trades` to get last market price

### Q4：Which timezone the start time of candlesticks falls into?
A: A： The start time for candlesticks is based on UTC

## Order and Trade

### Q1: How to ger the order params?
A: You can call Rest API `Post /api/v1/get_orderParas` to get the order params

# Contract Price API 

## Get Hopex Contract Data

```json
curl "https://api2.hopex.com/api/v1/ticker?contractCode=BTCUSDT"
```

`1. Get /api/v1/ticker`    rate limit 10 times/s

### Request Parameter

|Parameter|    Type|    Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String| yes |BTCUSDT ETHUSDT BTCUSD ETHUSD etc.|

> Response: 	

```json
{
  "data": {
    "contractCode": "BTCUSDT",
    "spotIndexCode": "spot_index_BTCUSDT",
    "fairPriceCode": "fair_price_BTCUSDT",
    "contractName": "BTC/USDT Swap",
    "closeCurrency": "USDT",
    "allowTrade": true,
    "pause": false,
   	"lastPrice": "-15372.0",
	"lastPriceToUSD": "$15380.45",
	"lastPriceLegal": "$15380.45",
	"changePercent24": "-0.54%",
	"marketPrice": "15373.85",
    "marketPriceInfo": "Average price of top 10 exchanges",
	"fairPrice": "15372.95",
    "fairePriceInfo": "Fair price=spot price+premium in last 15s. Unrealized PnL and liquidation price is calculated by fair price.",
	"price24Max": "15837.5",
	"price24Min": "14812.5",
	"amount24h": "860,725,245 USDT",
	"lastPriceToCNY": "￥101665.57",
	"quantity24h": "562031757",
	"fundRate": "+0.0015%"
  },
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1605003242400
}
```

### Return Value 	

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|contractCode|String| Contract Code|
|spotIndexCode|String| Spot Index Code|
|fairPriceCode|String| Fair Price Code|
|contractName|String| Contract Name|
|closeCurrency|String| Settlement Currency|
|allowTrade|bool| Whether the trade is applicable|
|pause|bool| whether to pause trade or not, Pause：true  Continue|
|lastPrice|String| The latest price|
|lastPriceToUSD|String| Latest USD Price|
|lastPriceLegal|String| Latest Fiat Price|
|changePercent24|String| 24h Rise or Fall |
|marketPrice|String| Spot Index Price|
|marketPriceInfo|String| Spot Index Price - Explanation|
|fairPrice|String| Fair Price|
|fairePriceInfo|String| Fair Price- Explanation|
|price24Max|String| 24h Highest Price|
|price24Min|String| Highest price in the last 24 hours|
|amount24h|String| Trading volume in the last 24 hours|
|lastPriceToCNY|String| the lastest CNY price|
|quantity24h|String| 24h Trading Quantity|

## Get Hopex Market Depth Information

> Request:

```json
"https://api2.hopex.com/api/v1/depth"
{
  "param": {
    "contractCode": "BTCUSDT"
  }
}
```

`2.Post /api/v1/depth`   rate limit 10 times/s

### Request Parameter   

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String| yes |BTCUSDT ETHUSDT BTCUSD ETHUSD etc|

> Response:		

```json
{
  "data": {
    "decimalplace": "1",
    "intervals": [
      "0.5"
    ],
	"asksFilter": "15540.1",
    "asks": [
      {
        "priceD": 15386.5000000000000000,
        "orderPrice": "15386.5",
        "orderQuantity": 29943,
        "orderQuantityShow": "29,943",
        "exist": 0
      },
      // ...
    ],
    "bidsFilter": "15232.4",
    "bids": [
      {
        "priceD": 15386.0000000000000000,
        "orderPrice": "15386.0",
        "orderQuantity": 33771,
        "orderQuantityShow": "33,771",
        "exist": 0
      },
      // ...
    ]
  },
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1605003762065
}     
```

### Return Value    

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|decimalplace|String| Precision|
|intervals|String| Interval|
|asksFilter|String| Highest Ask Price|
|asks|String| Asks|
|bidsFilter|String| Lowest Bid Price|
|bids|String| Bids|
|orderPrice|String| Order Price|
|orderQuantity|int| Quantity|
|exist|String| If there are traders’ orders placed at this price|

## Get Hopex latest completed orders

> Request:

```json
curl "https://api2.hopex.com/api/v1/trades?contractCode=BTCUSDT&pageSize=1"
```

`3. Get /api/v1/trades`   rate limit 10 times/s

### Request Parameter    

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String| yes |BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|pageSize|int|yes|the Number of Requests|

> Response:		

```json
{
  "data": [
	{
		"id": 538701017,
		"time": "18:19:04",
		"timestamp": 1605003544.5175519,
		"fillPrice": "15388.5",
		"fillQuantity": "3,100",
		"side": "2"
	}
  ],
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1605003762065
}
```

### Return Value

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|id|long|Latest trade Id|
|time|String| Order Complete Time|
|timestamp|float| Time Stamp|
|fillPrice|String| Deal Price|
|fillQuantity|String| Order Amount|
|side|String| Type 1 Sell 2 Buy|

## Get Hopex Kline data

> Request:

```json
curl "https://api2.hopex.com/api/v1/kline?contractCode=BTCUSDT&endTime=1603881210&startTime=1603814400&interval=1m"  
(Get Maximun 1000 Kline Data)
```

`4. Get /api/v1/kline`   rate limit 10 times/s

### Request Parameter

|Parameter|    Type|    Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|no|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|endTime|int64|yes|End Time-stamp(s)|
|startTime|int64|yes|Start Time-stamp(s)|
|interval|String|yes|interval mark, 1m->1min Kline 5m->5mins Kline 1h->1hr Kline 1d->1d Kline 1w->1week Kline 1M->1month Kline etc|

> Response:		

```json
{
    "data": {
        "decimalplace": "1",
        "timeData": [
            {
              "time": 1601481600,
              "open": "10773.5",
              "close": "10710.0",
              "high": "10915.0",
              "low": "10679.0",
              "vol": "309147297",
              "val": "668218402.577",
              "prevClose": "0.0",
              "upDown": "-63.50",
              "upDownRate": "-0.59%",
              "direct": 1,
              "contractValue": "0.0001"
            },
            //...
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605003708055
}
```

### Return Value

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|data|String| Kline Data|
|decimalplace|String| Decimal Places|
|time|long| Time Stamp(second)|
|open|String| The Opening Price|
|close|String| The Closing Price|
|high|String| The Highest Price|
|low|String| The Lowest Price|
|vol|String| Trading Volume|
|val|String| Turnover|
|prevClose|String| Latest Close Price|
|upDown|String| Rise and Fall|
|upDownRate|String| Change Rate|
|direct|int| Long or Short|
|contractValue|String| Contract Value|

## Get Hopex markets data

> Request:

```json
curl "https://api2.hopex.com/api/v1/markets"
```

`5. Get /api/v1/markets`   rate limit 10 times/s

> Response:		

```json
{
    "data": [
		{
			"sumAmount24hUSDT": 858243559.13565,
			"posVauleUSD": "18,377,012.65",
			"contractCode": "BTCUSDT",
			"contractName": "BTC/USDT Perpetual Swap",
			"allowTrade": true,
			"hasPosition": false,
			"closeCurrency": "USDT",
			"quotedCurrency": "USDT",
			"precision": 2,
			"minPriceMovement": 0.5,
			"pricePrecision": 1,
			"lastestPrice": 15383,
			"changePercent24h": -0.0037239726692788445970013924,
			"sumAmount24h": 858243559.13565
		},
        //...
	],
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1548160685910
}
```

### Return Value

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|contractCode|String| Contract Code|
|contractName|String| Contract Name|
|allowTrade|bool| Allow trade or not|
|hasPosition|bool| Own Position or not|
|closeCurrency|String| Settlement Currency|
|quotedCurrency|String| Quoted Currency|
|precision|int| Fair Price Precision|
|minPriceMovement|float| Tick Size|
|pricePrecision|int| Price Precision|
|lastestPrice|float| the Latest Price|
|changePercent24h|float| 24h Change|
|sumAmount24h：24h Trading Turnover in Settlement Currency|float|undefined|
|sumAmount24hUSDT|float| 24h Trading Turnover in USDT|
|posVauleUSD|String| Open Interest|

## Get Hopex Volume

> Request: 

```json
curl "https://api2.hopex.com/api/v1/indexStat"
```

`6. Get /api/v1/indexStat`  rate limit 10 times/s

> Response:	  

```json
{
  "data": {
         "posVauleUSD": "59,286,816.15",
        "posVauleCNY": "391,888,819.10",
        "amount24hUSD": "1,746,396,458.63",
        "amount24hCNY": "11,543,767,911.36",
        "amount7dayUSD": "11,595,850,460.39",
        "amount7dayCNY": "76,649,151,335.70",
        "userCount": "571,659",
        "dealCountUSD": "3,513,204,767,581.79",
        "dealCountCNY": "23,222,459,173,954.00"
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004042855
}
```

### Return Value  

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|posVauleUSD|String| Open Interest(USD)|
|posVauleCNY|String| Open Interest(CNY)|
|amount24hUSD|String| 24H Volume(USD)|
|amount24hCNY|String| 24H Volume(CNY)|
|amount7dayUSD|String| Trading volume in the last 7 days(USD)|
|amount7dayCNY|String| Trading volume in the last 7 days(CNY)|
|userCount|String| User Count|
|dealCountUSD|String| Total Trading volume(USD)|
|dealCountCNY|String| Total Trading volume(CNY)|

## Get Hopex Notifies

> Request:

```json
curl "https://api2.hopex.com/api/v1/index_notify?page=1&limit=5&culture=en"
```

`7. Get /api/v1/index_notify`   rate limit 10 times/s

### Request Parameter

|Parameter|    Type|    Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|page|int|no|Page No.,Default 1, Request Query|
|limit|int|no|Every Page Count,Default 20, Request Query|
|culture|String|no|Language(zh-CN,en,zh-HK),Default zh-CN,The number of announcements may be different in different languages,zh-CN prevail,Request Query|

> Response:	  

```json
{
    "data": {
        "totalCount": 11,
        "page": 1,
        "pageSize": 5,
        "result": [
        	{
				"id": 165,
				"title": "（5.28）System upgrade notification",
				"link": "https://h5.nineand.cn/notify/index.html?id=165",
				"lastModifiedTime": "2020-06-01 16:38:41",
				"time": "2020-06-01",
				"timestamp": 1591000721
			},
			{
				"id": 159,
				"title": "Hopex Acquired Canada MSB License, Accelerating the Pace of Globalization",
				"link": "https://h5.nineand.cn/notify/index.html?id=159",
				"lastModifiedTime": "2020-05-14 15:44:26",
				"time": "2020-05-14",
				"timestamp": 1589442266
			},
			//...
		]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605005434086
}
```

### Return Value   

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|totalCount|String| Hopex Notify Count|
|title|String| Notify Title|
|link|String| Notify Link|
|lastModifiedTime|String| Notify Last Update Time|
|timestamp|long| timestamp|


# Trading API

## Get Hopex user information

> Request:

```json
curl "https://api2.hopex.com/api/v1/userinfo"
```

`1. Get /api/v1/userinfo`    rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|

> Response:		

```json
{
    "data": {
		"conversionCurrency": "USD",
        "profitRate": "0.00%",
        "totalWealth": "1506.05",
        "floatProfit": "0.00",
        "position": 0,
        "activeOrder": 0
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004071676
}
```

### Return Value    

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|conversionCurrency|String|  Quote Currency|
|profitRate|String| Unrealized P/L Rate(Unrealized P/L/Position Margin)|
|totalWealth|String| Total Equity   (Quote Currency)|
|floatProfit|String| Floating P/L (Quote Currency)|
|position|int| Positions|
|activeOrder|int| Active Orders| 

## Place Orders

> Request: 

```json
"https://api2.hopex.com/api/v1/order"
{
  "param": {
    "contractCode": "BTCUSDT",
    "side": 2,
    "orderQuantity": 100,
    "orderPrice": 5255.5
  }
}
```

`2. Post /api/v1/order`    rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|no|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|side|int|no|1:Buy Long, 2:Sell Short, 3:Buy to Close Short, 4:Sell to Close Long|
|orderQuantity|int|yes|Order Quantity|
|orderPrice|number|no|integral multiple of tick size. If not filled, it means place a market order|

> Response:	

```json
{
    "data": 1950354706,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004071676
}
```

###Return Value    

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|ret|int| 0 represents successfully placed orders|
|data|long| Order ID| 



## Cancel Orders

> Request:

```json
curl "https://api2.hopex.com/api/v1/cancel_order?orderId=1952293255&contractCode=BTCUSDT"
```

`3. Get /api/v1/cancel_order`   rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|orderId|int|yes|Order id|
|contractCode|String|no|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|

> Response:		

```json
{
    "data": true,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004071676
}
```

### Return Value    

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|ret|int| return 0 and data is true means canceling is successful|
|data|bool| true means canceling is successful| 

## Get active orders information

> Request:

```json
curl "https://api2.hopex.com/api/v1/order_info?contractCode=BTCUSDT"
```

`4.Get /api/v1/order_info`   rate limit 1 time/s

### Request Parameter

|Parameter|    Type|    Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|no|if it is blank, it’s default to search for all active orders of all contracts|

> Response:	    

```json
{
    "data": [
        {
            "orderId": 11223769815,
            "orderType": "Buy long",
            "orderTypeVal": 1,
            "direct": 1,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT Perpetual Swap",
            "type": "1",
            "side": "2",
            "sideDisplay": "BUY",
			"ctime": "2020-11-10 15:21:43",
            "mtime": "2020-11-10 15:21:43",
            "orderQuantity": "+1,500",
            "leftQuantity": "1,500",
            "fillQuantity": "0",
            "orderStatus": "2",
            "orderStatusDisplay": "Pending",
            "orderPrice": "15190.0",
            "leverage": "20.00",
            "fee": "--",
            "avgFillMoney": "--",
            "orderMargin": "116.2035 USDT",
            "expireTime": "2020-11-17 15:21:43"
        }
    ],
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004441928
}   
```

### Return Value
|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|orderId|long| Order ID|
|orderType|String| Order Type 1:Buy Long, 2:Sell Short, 3:Buy to Close Short, 4:Sell to Close Long|
|orderTypeVal|int| Order Type 1:Buy Long, 2:Sell Short, 3:Buy to Close Short, 4:Sell to Close Long|
|direct|int|1 LONG,2SHORT|
|contractCode|String| Contract Code|
|contractName|String| Contract Name|
|type|String| 1.Limit Open2.Market Open 3.Limit Close 4.Market Close 5.Limit Partially Close 6.Market Partially Close|
|side|String| Buy or Sell, 1|
|sideDisplay|String| Direction|
|ctime|String| Creation Time|
|mtime|String| Update Time|
|orderQuantity|String| Quantity（contracts）|
|leftQuantity|String| Pending Quantity|
|fillQuantity|String| Complete Quantity|
|orderStatus|String| Order Status|
|orderStatusDisplay|String| Order Status|
|orderPrice|String| Order Price|
|leverage|String| Leverages（2 decimals）|
|fee|String| Trading Fee(4 decimals)|
|avgFillMoney|String| Average Deal Price(Index_Fair Price Decimals)|
|orderMargin|String| Order Margin(4 decimals)|
|expireTime|String| Expire Time| 

## Get History Orders

> Request:

```json
"https://api2.hopex.com/api/v1/order_history?page=1&limit=10"
{
  "param": {
    "contractCodeList": [
      
    ],
    "typeList": [
      
    ],
    "side": 0,
    "startTime": 0,
    "endTime": 0
  }
}
```

`5. Post /api/v1/order_history`    rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCodeList|String[]|yes|Contract List, Being blank to search all contracts, Request Body|
|typeList|int[]|yes|1.Limit Price to Open 2.Market Price to Open 3.Limit Price to Close 4.Market Price to Close 5.Limit Price Close Partially Complete 6.Market Price Close Partially Complete, Request Body
|side|int|yes|0:no limit 1 for sell, 2 for buy, Request Body|
|startTime|int|yes|Start Time Stamp, Request Body|
|endTime|int|yes|End Time Stamp, Request Body|
|page|int|yes|Page No.,Default 1, Request Query|
|limit|int|no|Every Page Count,Default 10, Request Query|

> Response:	

```json
{
    "data": {
        "totalCount": 0,
        "page": 2,
        "pageSize": 10,
        "result": [
            {
                "orderId": 11223486806,
                "contractCode": "BTCUSDT",
                "contractName": "BTC/USDT Perpetual Swap",
                "type": "4",
                "side": "1",
                "direct": 2,
                "sideDisplay": "Sell",
                "ctime": "2020-11-10 15:03:55",
                "ftime": "2020-11-10 15:03:55",
                "orderQuantity": "-1,000",
                "fillQuantity": "-1,000",
                "orderStatus": "2",
                "orderStatusDisplay": "Order Complete",
                "orderPrice": "Market Price",
                "leverage": "20.00",
                "fee": "0.7650 USDT",
                "avgFillMoney": "15299.50",
                "closePosPNL": "+1.6500 USDT",
                "timestamp": 1604991835266679,
                "orderTypeVal" : 2,
                "orderType": "Sell to Open Short",
                "cancelReason": "Cancel Reason"
            },
            //...
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004540319
}      
```    

### Return Value	

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|orderId|long| Order ID|
|contractCode|String| Contract Code|
|contractName|String| Contract Name|
|type|String| 1.Limit Order 2.Market Order 3.Limit Price to Close 4.Market Price to Close|
|side|String| Buy or Sell, 1.Sell 2.Buy|
|direct|int| Positions 1:Long 2:Short|
|sideDisplay|String| Direction|
|ctime|String| Creation Time|
|ftime|String| Complete Time|
|orderQuantity|String| Quantity（contracts）|
|fillQuantity|String| Complete Quantity|
|orderStatus|String| Order Status|
|orderStatusDisplay|String| Order Status|
|orderPrice|String| Order Price|
|leverage|String| Leverages（2 decimals）|
|fee|String| Trading Fee(4 decimals)|
|avgFillMoney|String| Average Deal Price(Index_Fair Price Decimals)|
|closePosPNL|String| Realized P/L 4 decimals|
|timestamp|long| Time Stamp(Microsecond)|
|orderTypeVal|String| 1|
|orderType|String| Buy to Open Long, Sell to Open Short, Buy to Close Short, Sell to Close Short|
|cancelReason|String| Cancel Reason| 


## Get positions information

> Request:

```json
curl "https://api2.hopex.com/api/v1/position"
```

`6. Get /api/v1/position`    rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|no|Contract List|

> Response:	

```json
# Request
GET https://api2.hopex.com/api/v1/position

# Response
{
    "data": [
        {
            "allowFullClose": true,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT Perpetual Swap",
            "leverage": "20.00",
            "contractValue": "0.0001",
            "maintMarginRate": "0.005",
            "takerFee": "0.001",
            "positionQuantity": "+100",
            "direct": 2,
            "posiDirect": 1,
            "posiDirectD": "Long Positions",
			"entryPrice": "15299.00",
            "entryPriceD": 15299,
            "positionMargin": "200.6831 USDT",
            "positionMarginD": 200.68311,
            "liquidationPrice": "14229.02",
            "maintMargin": "0.3133 USDT",
            "unrealisedPnl": "-0.0025 USDT",
            "unrealisedPnlPcnt": "-0.09%",
            "fairPrice": "15299.97",
            "fairPriceD": 15299.97,
            "lastPrice": "15299.5",
            "sequence": 0,
            "rank": 0,
            "minPriceMovement": 0.5,
            "minPriceMovementPrecision": 1,
            "positionQuantityFreeze": "0",
            "closeablePositionQuantity": "200",
            "isAddMargin": false,
            "sort": 9999,
            "isWhitelistExist": true,
            "closeCurrency": "USDT"
        }
    ],
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555582208144
}
```

### Return Value    

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|allowFullClose|bool| Allow Full Close|
|contractCode|String| Contract Code|
|contractName|String| Contract Name|
|leverage|String| Leverages|
|contractValue|String| Contract Value|
|maintMarginRate|String| Maintenance Margin Rate |
|takerFee|String| Taker Fee|
|positionQuantity|String| Position Quantity|
|direct|int| Long or Short 1.Long 2.Short|
|posiDirect|int| Positions（Long 1 /Short -1)|
|posiDirectD|String| Positions（Long/Short)|
|entryPrice|String| Average Open Price|
|entryPriceD|float| Average Open Price|
|positionMargin|String| Position Margin|
|positionMarginD|float| Position Margin|
|liquidationPrice|String| Liquidation Price|
|maintMargin|String| Maintenance Margin|
|unrealisedPnl|String| Unrealized P/L|
|unrealisedPnlPcnt|String| Unrealized P/L Rate|
|fairPrice|String| Fair Price|
|fairPriceD|float| Fair Price|
|lastPrice|String| the Latest Price|
|sequence|int| Sequence Length|
|rank|int| Rank|
|minPriceMovement|float| Tick Size|
|minPriceMovementPrecision|int| Tick Size Precision|
|positionQuantityFreeze|String| Frozen Positions,Total number of close orders that are in pending|
|closeablePositionQuantity|String| Quantity of  existing positions that can be closed|
|isAddMargin |bool| Set Auto Add Margin|
|sort|int| sort|
|isWhitelistExist|bool| Exist In Whitelist|
|closeCurrency|String| Settlement currency|

## Place Condition Orders

> Request:

```json
"https://api2.hopex.com/api/v1/condition_order"
{
  "param": {
    "contractCode": "BTCUSDT",
    "side": 1,
    "type": "Limit",
    "trigPrice": 14022.5,
    "expectedQuantity": 1000,
    "expectedPrice": 12811.5
  }
}
```

`7. Post /api/v1/condition_order`   rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|yes|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|side|int|yes|1:Buy Long, 2:Sell Short, 3:Buy to Close Short, 4:Sell to Close Long|
|type|String|yes|Limit:limit order Market:market order|
|trigPrice|number|yes|Trigger price|
|expectedQuantity|int|yes|Preset quantity|
|expectedPrice|number|yes|Preset price|

> Response:	  

```json
{
    "data": true,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1603692543034
}
```

### Return Value   

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|ret|int| 0 represents successfully placed condition orders|
|data|bool| true means placing order is successful| 

## Get condition orders infomation

> Request:

```json
"https://api2.hopex.com/api/v1/condition_order_info?page=1&limit=10"
{
  "param": {
    "contractCodeList": ["BTCUSDT"],
    "taskTypeList": [],
    "trigTypeList": [],
    "taskStatusList": [],
    "direct": 0,
    "side": 0,
    "startTime": 0,
    "endTime": 0
  }
}
```

`8. Post /api/v1/condition_order_info`    rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCodeList|String[]|no|Contract List, Being blank to search all contracts, Request Body|
|taskTypeList|int[]|no|1:Buy Long, 2:Sell Short, 3:Buy to Close Short, 4:Sell to Close Long, Being blank to search all, Request Body|
|trigTypeList|int[]|no|1:Market Price 2:Faire Price,Being blank to search all, Request Body|
|taskStatusList|int[]|no|1: Untriggered 2.Canceled 3.Order Submitted 4.Trigger failed, Being blank to search all, Request Body|
|direct|int|yes|1 LONG,2 SHORT,0:search all|
|side|int|yes|Buy or Sell, 1:Sell 2Buy,0:search all|
|startTime|number|yes|0:search all,Start Time Stamp(Unit microsecond)|
|endTime|number|yes|0:search all,End Time Stamp(Unit microsecond)|

> Response:	  

```json
{
    "data": {
        "totalCount": 2,
        "page": 1,
        "pageSize": 10,
        "result": [
            {
                "taskType": 1,
                "taskTypeD": "Open long",
                "taskId": 207565,
                "contractCode": "BTCUSDT",
                "contractName": "BTC/USDT Swap",
                "action": 0,
                "direct": 1,
                "side": 2,
                "taskStatus": 1,
                "taskStatusD": "Untriggered",
                "trigType": 1,
                "trigTypeD": "MP>=14000.00",
                "trigPrice": "14000.00",
                "expectedQuantity": "+1,000",
                "expectedPrice": "12800.0",
                "expireTime": "2020-11-02 14:09:03",
                "timestamp": 1.60369254302209E+15,
                "createTime": "2020-10-26 14:09:03",
                "orderId": 0,
                "orderQuantity": "--",
                "orderPrice": "--",
                "finishTime": "--",
                "failureReason": "Created",
                "leverage": "30.00"
            },
            //...
            {
                "taskType": 1,
                "taskTypeD": "Open long",
                "taskId": 207564,
                "contractCode": "BTCUSDT",
                "contractName": "BTC/USDT Swap",
                "action": 0,
                "direct": 1,
                "side": 2,
                "taskStatus": 1,
                "taskStatusD": "Untriggered",
                "trigType": 1,
                "trigTypeD": "MP<=12000.00",
                "trigPrice": "12000.00",
                "expectedQuantity": "+1,000",
                "expectedPrice": "12800.0",
                "expireTime": "2020-11-02 14:06:50",
                "timestamp": 1.60369241004107E+15,
                "createTime": "2020-10-26 14:06:50",
                "orderId": 0,
                "orderQuantity": "--",
                "orderPrice": "--",
                "finishTime": "--",
                "failureReason": "Created",
                "leverage": "30.00"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1603696164355
}
```

### Return Value    

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|taskType|int|Condition task type,1:Buy Long, 2:Sell Short, 3:Buy to Close Short, 4:Sell to Close Long|
|taskId|long|Condition task id|
|contractCode|String|Contract Code|
|contractName|String|Contract Name|
|action|int|1.Open 2.Close|
|direct|int|1 LONG,2 SHORT|
|side|String|Buy or Sell, 1.Sell 2.Buy|
|taskStatus|int|Condition task status，1: Untriggered 2.Canceled 3.Order Submitted 4.Trigger failed|
|trigType|int|1.market price 2.faire price|
|trigPrice|String|Trigger Price|
|expectedQuantity|String|Preset quantity|
|expectedPrice|String|Preset price|
|expireTime|String|Expired Time|
|orderId|long|Order ID|
|orderQuantity|String|Quantity（contracts）|
|orderPrice|String|Order Price|
|leverage|String|Leverages（2 decimals）| 

## Cancel Condition Orders

> Request:

```json
"https://api2.hopex.com/api/v1/cancel_condition_order"
{
  "param": {
    "contractCode": "BTCUSDT",
    "taskId": 207565
  }
}
```

`9. Post /api/v1/cancel_condition_order`    rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|yes|BTCUSDT ETHUSDT BTCUSD ETHUSD etc|
|taskId|int|yes|condition task Id|

> Response:	  

```json
{
    "data": true,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1603698662317
}
```

### Return Value    

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|ret|int| return 0 and data is true means canceling condition order is successful|
|data|bool| true means canceling condition order is successful| 

## Get asset information

> Request:

```json
curl "https://api2.hopex.com/api/v1/wallet"
```

`10. Get /api/v1/wallet`   rate limit 1 time/s

### Request Parameter

|Parameter|    Type|    Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|

> Response:	

```json
{
    "data": {
        "summary": {
            "conversionCurrency": "USD",
            "totalWealth": "0.78",
            "floatProfit": "0.00",
            "availableBalance": "0.78"
        },
        "detail": [
            {
                "assetName": "USDT",
                "assetLogoUrl": null,
                "floatProfit": "0.00000000",
                "floatProfitLegal": "0.00 USD",
                "profitRate": "0.00% ",
                "totalWealth": "0.77484596",
                "totalWealthLegal": "0.78 USD",
                "totalWealthInfo": "Total equity = wallet balance + unrealized profit and loss",
                "availableBalance": "0.77484596",
                "availableBalanceLegal": "0.78 USD",
                "availableBalanceInfo": "Available Balance = Wallet Balance - Position Margin - Order Margin - Withdrawal Amount",
                "walletBalance": "0.77484596",
                "walletBalanceLegal": "0.78 USD",
                "walletBalanceInfo": "Wallet Balance = Deposit - Withdraw+ Closing Profit and Loss - Trading Fee",
                "positionMargin": "0.00000000",
                "positionMarginLegal": "0.00 USD",
                "delegateMargin": "0.00000000",
                "delegateMarginLegal": "0.00 USD",
                "withdrawFreeze": "0.00000000",
                "withdrawFreezeLegal": "0.00 USD",
                "depositAmount": "1266.87475194",
                "depositAmountLegal": "1268.27 USD",
                "withdrawAmount": "1231.50005415",
                "withdrawAmountLegal": "1232.85 USD"
            },
            {
                "assetName": "BTC",
                "assetLogoUrl": null,
                "floatProfit": "0.00000000",
                "floatProfitLegal": "0.00 USD",
                "profitRate": "0.00% ",
                "totalWealth": "0.00000000",
                "totalWealthLegal": "0.00 USD",
                "totalWealthInfo": "Total equity = wallet balance + unrealized profit and loss",
                "availableBalance": "0.00000000",
                "availableBalanceLegal": "0.00 USD",
                "availableBalanceInfo": "Available Balance = Wallet Balance - Position Margin - Order Margin - Withdrawal Amount",
                "walletBalance": "0.00000000",
                "walletBalanceLegal": "0.00 USD",
                "walletBalanceInfo": "Wallet Balance = Deposit - Withdraw+ Closing Profit and Loss - Trading Fee",
                "positionMargin": "0.00000000",
                "positionMarginLegal": "0.00 USD",
                "delegateMargin": "0.00000000",
                "delegateMarginLegal": "0.00 USD",
                "withdrawFreeze": "0.00000000",
                "withdrawFreezeLegal": "0.00 USD",
                "depositAmount": "0.00085151",
                "depositAmountLegal": "5.91 USD",
                "withdrawAmount": "0.00085151",
                "withdrawAmountLegal": "5.91 USD"
            },
            {
                "assetName": "ETH",
                "assetLogoUrl": null,
                "floatProfit": "0.00000000",
                "floatProfitLegal": "0.00 USD",
                "profitRate": "0.00% ",
                "totalWealth": "0.00000000",
                "totalWealthLegal": "0.00 USD",
                "totalWealthInfo": "Total equity = wallet balance + unrealized profit and loss",
                "availableBalance": "0.00000000",
                "availableBalanceLegal": "0.00 USD",
                "availableBalanceInfo": "Available Balance = Wallet Balance - Position Margin - Order Margin - Withdrawal Amount",
                "walletBalance": "0.00000000",
                "walletBalanceLegal": "0.00 USD",
                "walletBalanceInfo": "Wallet Balance = Deposit - Withdraw+ Closing Profit and Loss - Trading Fee",
                "positionMargin": "0.00000000",
                "positionMarginLegal": "0.00 USD",
                "delegateMargin": "0.00000000",
                "delegateMarginLegal": "0.00 USD",
                "withdrawFreeze": "0.00000000",
                "withdrawFreezeLegal": "0.00 USD",
                "depositAmount": "0.07003782",
                "depositAmountLegal": "11.12 USD",
                "withdrawAmount": "0.07003782",
                "withdrawAmountLegal": "11.12 USD"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004744343
}
```

###Return Value

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|conversionCurrency|String| Quote Currency|
|totalWealth|String| Total Equity|
|totalWealthLegal|String| Total Equity (USD)|
|floatProfit|String| Floating P/L|
|floatProfitLegal|String| Floating P/L (USD)|
|availableBalance|String| Available Balance|
|availableBalanceLegal|String| Available Balance (USD)|
|assetName|String| Asset Name|
|assetLogoUrl|String| Asset Logo|
|profitRate|String| Profit Rate|
|walletBalance|String| Wallet Balance|
|walletBalanceLegal|String| Wallet Balance (USD)|
|positionMargin|String| Position Margin|
|positionMarginLegal|String| Position Margin (USD)|
|delegateMargin|String| Delegate Margin|
|delegateMarginLegal|String| Delegate Margin (USD)|
|withdrawFreeze|String| Withdraw Freeze Amount|
|withdrawFreezeLegal|String| Withdraw Freeze Amount (USD)|
|depositAmount|String| Deposit Amount|
|depositAmountLegal|String| Deposit Amount (USD)|
|withdrawAmount|String| Withdraw Amount|
|withdrawAmountLegal|String| Withdraw Amount (USD)| 

## Set Contract Leverage

> Request:

```json
curl "https://api2.hopex.com/api/v1/set_leverage?contractCode=BTCUSDT&direct=2&leverage=70"
```

`11. Get /api/v1/set_leverage`    rate limit 1 time/s

### Request Parameter

|Parameter|    Type|    Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|yes|Contract Name|
|direct|int|yes|Long or Short:1 Long,2 Short|
|leverage|number|yes|Leverage|

> Response:		

```json
{
    "data": 70.00,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004744343
}
```

### Return Value

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|data|float| Leverage (2 Decimals)|

## Get Order Parameter

> Request:

```json
"https://api2.hopex.com/api/v1/get_orderParas"
{
  "param": {
    "contractCode": "BTCUSDT"
  }
}
```

`12. Post /api/v1/get_orderParas`   rate limit 1 time/s

### Request Parameter

|Parameter|Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCode|String|yes|BTCUSDT ETHUSDT BTCUSD ETHUSD...|

> Response:	  

```json
# Request
{
  "data": {
    "contractCode": "BTCUSDT",
    "contractDirect": "Forward",
    "contractValue": "0.0001",
    "valueUnit": "BTC",
    "closeCurrency": "USDT",
    "takeRate": null,
    "userAllowTrade": false,
    "marketAllowTrade": true,
    "minPricePrecision": 1,
    "minPriceMovement": "0.5",
    "minPriceMovementDisplay": "0.5 USDT",
    "longMaintenanceMarginRate": "0.005",
    "longMaintenanceMarginRateDisplay": "0.5%",
    "shortMaintenanceMarginRate": "0.005",
    "shortMaintenanceMarginRateDisplay": "0.5%",
    "minTradeNum": 1,
    "minTradeNumDisplay": "1 piece",
    "availableBalance": "1389.0207",
	"availableBalanceDisplay": "1389.0207 USDT",
	"maxBuyPrice": "15881.0",
	"minSellPrice": "14956.0",
	"longMinLeverage": "2.00",
	"longMaxLeverage": "100.00",
	"shortMinLeverage": "2.00",
	"shortMaxLeverage": "100.00",
	"longDefaultLeverage": "100.00",
	"shortDefaultLeverage": "100.00",
	"longLeverage": "20.00",
	"shortLeverage": "20.00",
	"openLongMargin": "0.0000",
	"openLongMarginDisplay": "0.0000 USDT",
	"openShortMargin": "0.0000",
	"openShortMarginDisplay": "0.0000 USDT",
	"openLongAmount": 17663,
	"openShortAmount": 17663,
	"closeLongAmount": 0,
	"closeShortAmount": 0,
	"evaluateOrderValue": "0.0000 BTC",
	"precision": 2
  },
  "ret": 0,
  "errCode": null,
  "errStr": null,
  "env": 0,
  "timestamp": 1605004823093
}
```

### Return Value   

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|longMinLeverage|String| Long Leverage Min|
|longMaxLeverage|String| Long Leverage Max|
|shortMinLeverage|String| Short Leverage Min|
|shortMaxLeverage|String| Short Leverage Max| 

## Get liquidation history

> Request:

```json
"https://api2.hopex.com/api/v1/liquidation_history"
{
  "param": {
    "contractCodeList": [
      
    ],
    "side": 0
  }
}
```

`13. Post /api/v1/liquidation_history`    rate limit 1 time/s

### Request Parameter

|Parameter|Type|Required|Description|
|:-----|:-----|:-----|:-----|
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|contractCodeList|String[]|yes|Contract List, leave it black to search all contracts|
|side|int|yes|0:no limit 1 for sell, 2 for buy.|
|page|int|yes|Page No., Default 1|

> Response:	

```json
{
    "data": {
        "totalCount": 7,
        "page": 1,
        "pageSize": 10,
        "result": [
            {
                "orderId": 7,
                "contractCode": "ETHUSDT",
                "contractName": "ETH/USDT Perpetual Swap",
                "side": "2",
                "sideDisplay": "Buy",
                "orderType": "Buy to Close Short",
                "orderTypeVal": 3,
                "direct": 2,
                "leverage": "20.00",
                "orderQuantity": "+70,731",
                "orderPrice": "194.43",
                "closePosPNL": "-6545.5086 USDT",
                "fee": "68.7606 USDT",
                "ctime": "2020-08-02 12:39:18",
                "timestamp": 1596343158162222,
                "direction": 1,
                "directionDisplay": "Short",
                "positionMargin": "+6614.2692 USDT",
                "openPrice": "185.17",
                "liquidationPriceReal": "193.50",
                "showDetail": false
            },
        	//...
            {
                "orderId": 2,
                "contractCode": "EOSUSDT",
                "contractName": "EOS/USDT Perpetual Swap",
                "side": "1",
                "sideDisplay": "Sell",
                "orderType": "Sell to Close Long",
                "orderTypeVal": 4,
                "direct": 1,
                "leverage": "20.00",
                "orderQuantity": "-1,123",
                "orderPrice": "6.4883",
                "closePosPNL": "-383.6963 USDT",
                "fee": "3.6432 USDT",
                "ctime": "2020-08-02 12:39:18",
                "timestamp": 1596343158162222,
                "direction": 2,
                "directionDisplay": "Long",
                "positionMargin": "+387.3395 USDT",
                "openPrice": "6.8300",
                "liquidationPriceReal": "6.5567",
                "showDetail": false
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004871187
}
```		

### Return Value	

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|orderId|long| Order ID|
|contractCode|String| Contract Code|
|contractName|String| Contract Name|
|side|int| Buy or Sell, 1.Sell 2.Buy|
|sideDisplay|String| Buy or Sell|
|orderType|String| 1.Buy to Open Long, 2.Sell to Open Short, 3.Buy to Close Short, 4.Sell to Close Long|
|orderTypeVal|int| 1.Buy to Open Long, 2.Sell to Open Short, 3.Buy to Close Short, 4.Sell to Close Long|
|direct|int| Long or Short 1.Long 2.Short|
|leverage|String| Leverage（2 Decimals）|
|orderQuantity|String| Order Quantity（contracts）|
|orderPrice|String| Order Price|
|closePosPNL|String| Realized P/L, 4 Decimals|
|fee|String| Trading Fee(4 Decimals)|
|ctime|String| Creation Time|
|timestamp|long| Time Stamp(microsecond)|
|direction|int| 1.Short 2.Long|
|positionMargin|String| Position Margin (4Decimals)|
|openPrice|String| Average Open Price|
|liquidationPriceReal|String| Liquidation Price|
|showDetail|bool| Ignore| 

## Get deposit and withdraw history

> Request:

```json
curl "https://api2.hopex.com/api/v1/account_records?page=1&limit=10"
```

`14. Get /api/v1/account_records`    rate limit 1 time/s

### Request Parameter

|Parameter| Type|   Required|   Description|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|yes|User Information Verification, Request Header|
|Date|String|yes|Current GMT Time, Request Header|
|Digest|String|yes|Request for Package Summary, Request Header|
|page|int|no|Page No., Default 1|
|limit|int|no|the number of returned information on each page, Default 20|

> Response:	

```json
{
    "data": {
        "totalCount": 8,
        "page": 1,
        "pageSize": 10,
        "result": [
            {
        	    "id": 58413,
                "asset": "USDT",
                "orderType": 4,
                "orderTypeD": "Onchian Withdraw",
                "amount": "-1900.00000000 USDT",
                "rmbAmount": "0.00 CNY",
                "bankName": "",
                "addr": "mvYW73ZTo8F7aksBrWad3XvqViqDE3saaP",
                "orderStatus": 1,
                "orderStatusD": "Complete",
                "createdTime": "2020-11-10 16:13:00"
            },
            //...
            {
	    	    "id": 58421,
                "asset": "USDT",
                "orderType": 2,
                "orderTypeD": "OTC Withdraw",
                "amount": "-100.00000000 USDT",
                "rmbAmount": "704.00 CNY",
                "bankName": "Postal Savings Bank of China",
                "addr": "",
                "orderStatus": 1,
                "orderStatusD": "Complete",
                "createdTime": "2020-11-10 09:57:18"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1605004929900
}
```
### Return Value	

|Parameter| Type| Description|
| :-----    | :-----   | :-----    |
|asset|String| Asset Name|
|orderType|int| 1 OTC Deposit,2 OTC Withdraw,3 Onchain Deposit,4 Onchain Withdraw,5 Internal Transfer-Deposit,6 Internal Transfer-Withdraw, 7 Manual Deposit, 8 Manual Withdraw,9 Quick Deposit,10 Quick Withdraw|
|orderTypeD|String| Order Type Description|
|amount|String| Asset Amount|
|rmbAmount|String| RMB Amount|
|bankName|String| Bank Name|
|addr|String| Wallet Address|
|orderStatus|int| 0 Pending, 1 Complete, 2 Failed|
|orderStatusD|String| Order Status Description|
|createdTime|String| Creation Time| 


# API Authentication

## Authentication Details

The authentication details include the following four aspects:

### Generate API Key

Your API Key and API Secret must be generated through Hopex. API Key and API Secret will be randomly generated and provided by Hopex.

### Initiate a request

All private REST request headers must include Authorization, Date and Digest.

### Signature

Authorization header has following format: -H 'Authorization: hmac apikey = "...", algorithm = "hmac-sha256", headers = "date request-line digest", signature = "..."'. apikey is Your API Key, signature is method + request-line + "HTTP / 1.1" + digest, encrypt with HMAC SHA256 method by secret, and obtain through BASE64 encoded output. request-line is the request path. like /api /v1/userinfo, digest is the digest of the request body.

### GMT time
The Date request header must be the standard Greenwich Mean Time format corresponding to the time of the request, such as Fri, 19 Apr 2019 03:15:20 GMT, if the request is sent more than 1 minute away from the server time, the system will consider the request to be expired and refused.

## Sample Program

Here is a sample program of generating Authorization, Date and Digest Request Header:

``` javascript
var apiKey = "..."; // your api key
var apiSecret = "..."; // your applied secret key

var algorithm = "hmac-sha256";
var head_auth_headers = "date request-line digest";

var date = new Date().toGMTString();
pm.globals.set("Date", date);

var data = request.data;
var Digest = CryptoJS.SHA256(data).toString(CryptoJS.enc.Base64);
pm.globals.set('Digest', "SHA-256=" + Digest);

var textToSign = "";
textToSign += "date: " + date + "\n";
textToSign += request.method + " " + "/api/v1/userinfo" + " HTTP/1.1" + "\n";
textToSign += "digest: " + "SHA-256=" + Digest;
console.log("textToSign:\n" + textToSign.replace(/\n/g, "#"));
var signature = CryptoJS.HmacSHA256(textToSign, apiSecret).toString(CryptoJS.enc.Base64);
var head_auth = "hmac apikey=\"" + apiKey + "\", algorithm=\"" + algorithm  + "\", headers=\"" + head_auth_headers + "\", signature=\"" + signature + "\"";
pm.globals.set("Authorization",  head_auth);
``` 
## Sample Demo

Here is a sample of using Postman tool to get History Orders:

![Hopex Img](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/10-250-637393902050348151.png)

![Hopex Img](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/11-223-637393902082612713.png)

![Hopex Img](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/12-245-637393902111464004.png)

![Hopex Img](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/13-168-637393902133510542.png)



