---
title: Hopex API 

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='https://web.hopex.com/myaccount?page=ApiManage'>创建API Key</a>

includes:

search: true

code_clipboard: false
---

# 简介

## API简介 

欢迎使用Hopex API！

此文档是HopexAPI的唯一官方文档，汇贝API提供的功能和服务会在此持续更新，请大家及时关注。

通过点击右上方的语言按钮来切换文档语言。

文档右侧是请求参数以及响应结果的示例。
    
## 请求交互    

REST访问的根URL：`https://api2.hopex.com/api/v1/` 

所有请求基于Https协议
	
请求交互说明    
1. 请求参数：根据接口请求参数规定进行参数封装。    
2. 提交请求参数：将封装好的请求参数通过POST 或GET 方式提交至服务器。    
3. 服务器响应：服务器首先对用户请求数据进行参数安全校验,通过校验后根据业务逻辑将响应数据以JSON格式返回给用户。    
4. 数据处理：对服务器响应数据进行处理。 

# 接入说明

## 接口预览

|请求方式| 请求Url | 描述|
| :-----    | :-----   | :-----    |
|Get| [/api/v1/ticker](#hopex) | 获取Hopex合约行情 |
|Post| [/api/v1/depth](#hopex-2) | 获取Hopex合约深度信息 |
|Get| [/api/v1/trades](#hopex-3) | 获取Hopex新成交信息 |
|Get| [/api/v1/kline](#hopex-k) | 获取Hopex K线 |
|Get| [/api/v1/markets](#hopex-4) | 获取Hopex 所有合约行情 |
|Get| [/api/v1/indexStat](#hopex-5) | 获取Hopex成交量 |
|Get| [/api/v1/index_notify](#hopex-6) | 获取Hopex交易所推送公告 |
|Get| [/api/v1/userinfo](#hopex-7) | 获取Hopex用户信息 |
|Post| [/api/v1/order](#73292b0f57) | 用户下单 |
|Get| [/api/v1/cancel_order](#11d9e25be8) | 用户撤单 |
|Get| [/api/v1/order_info](#51eddc5aa9) | 获取用户的活跃委托 |
|Post| [/api/v1/order_history](#4212d667d1) | 获取历史委托 |
|Get| [/api/v1/position](#94eccbb1b8) | 获取持仓 |
|Post| [/api/v1/condition_order](#f9e87fb381) | 下条件单 |
|Post| [/api/v1/condition_order_info](#4d2cea97f7) | 查询条件单列表 |
|Post| [/api/v1/cancel_condition_order](#6a6d683558) | 用户撤条件单 |
|Get| [/api/v1/wallet](#224168edc1) | 获取资产信息 |
|Get| [/api/v1/set_leverage](#24da12c248) | 设置用户合约杠杆 |
|Post| [/api/v1/get_orderParas](#ed645b50df) | 获取下单参数 |
|Post| [/api/v1/liquidation_history](#17307ac747) | 获取强平历史 |
|Get| [/api/v1/account_records](#8e62101ca9) | 获取用户出入金记录 |

## 请求格式

所有的API请求都是restful，目前只有两种方法：GET和POST

* GET请求：所有的参数都在路径参数里
* POST请求，所有参数以JSON格式发送在请求主体（body）里
* 需要密钥验证的请求需在请求头(Header)添加 `Authorization`,`Date`,`Digest`,验证细节参考[API验签](#c81b9fb21f)

## 返回格式

所有的接口都是JSON格式。

```json
{
  "ret": 0,
  "errCode": null,
  "errStr": null,
  "timestamp": 1604975001875,
  "data": // per API response data in nested JSON object
}
```

|参数名称| 数据类型 | 描述|
| :-----    | :-----   | :-----    |
|ret       | int    | API接口返回状态, 0:正常, -1:异常|
|errCode   | string    | 接口数据返回的错误编码|
|errStr    | string   | 接口返回的错误信息|
|data      | object    | 接口返回数据主体|
|timestamp | long    | 接口返回的UTC时间的时间戳，单位毫秒 |

## 数据类型

本文档对JSON格式中数据类型的描述做如下约定：

- `string`: 字符串类型，用双引号（"）引用
- `int`: 32位整数，主要涉及到状态码、大小、次数等
- `long`: 64位整数，主要涉及到Id和时间戳
- `float`: 浮点数，主要涉及到金额和价格，建议程序中使用高精度浮点型
- `bool`: 布尔值，主要涉及到是否暂停交易

# 常见问题

## 接入、验签相关

### Q1: 一个用户可以申请多少个Api Key?
A: 每个用户可以创建一个交易Api Key和一个只读Api Key

### Q2: 交易Api Key跟只读Api Key有什么区别?
A: 只读Api Key无法访问下单、撤单接口，其他查询接口均可使用，交易Api Key则可以访问所有接口

### Q3: 访问接口返回"HMAC signature does not match"
A: 请检查是否属于以下情况：

1. 访问接口使用的Api Key跟Api Secret不正确

2. 访问接口时请求头签名内标明的接口路径与访问的接口路径不一致

### Q4: 访问接口返回"API rate limit exceeded"
A: 访问接口频率太频繁

## 行情相关

### Q1: 当前盘口数据多久刷新一次?
A: 当前盘口数据实时刷新

### Q2: 接口获取24小时交易额或交易量会出现负增长问题
A: 接口中的交易额或交易量为24小时滚动数据 (平移窗口大小24小时)，有可能会出现一个窗口内的累计交易量、累计交易额小于前一窗口的情况

### Q3: 如何获取最新成交价格?
A: 推荐使用REST API `GET /api/v1/trades` 接口请求最新成交

### Q4：K线是按照什么时间开始计算的?
A: K线周期以UTC时间为基准开始计算

## 交易相关

### Q1: 如何获取下单参数的信息?
A: 可使用 Rest API `Post /api/v1/get_orderParas` 获取下单参数的信息

# 合约行情 API 

## 获取Hopex合约行情  

> Request:

```json
curl "https://api2.hopex.com/api/v1/ticker?contractCode=BTCUSDT"
```

`1. Get /api/v1/ticker`   访问频率 10次/秒

### 请求参数    

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|

> Response:	

```json
{
  "data": {
    "contractCode": "BTCUSDT",
    "spotIndexCode": "spot_index_BTCUSDT",
    "fairPriceCode": "fair_price_BTCUSDT",
    "contractName": "BTC/USDT永续",
    "closeCurrency": "USDT",
    "allowTrade": true,
    "pause": false,
    "lastPrice": "-3520.0",
    "lastPriceToUSD": "$3522.11",
    "lastPriceLegal": "$3522.11",
    "changePercent24": "-0.21%",
    "marketPrice": "3518.86",
    "marketPriceInfo": "全球top10成交量交易所均价",
    "fairPrice": "3519.05",
    "fairePriceInfo": "合理价格=现货价格+最近15秒成交溢价,此价格可以更加合理地代表Hopex合约价格。浮动盈亏和强平价格均是根据合理价格来计算。",
    "price24Max": "3551.5",
    "price24Min": "3494.5",
    "amount24h": "3,735,255 USDT",
    "lastPriceToCNY": "￥26242.47",
    "quantity24h": "13,736,960"
  },
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1548150572035
}
```

### 返回值说明

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|contractCode|String|合约编码|
|spotIndexCode|String|现货指数编码|
|fairPriceCode|String|合理价格编码|
|contractName|String|合约名称|
|closeCurrency|String|结算货币|
|allowTrade|bool|是否允许交易|
|pause|bool|是否暂停交易,暂停：true 启用: false|
|lastPrice|String|最新价|
|lastPriceToUSD|String|最新价To USD|
|lastPriceLegal|String|最新价To 法币|
|changePercent24|String|24小时涨跌幅|
|marketPrice|String|现货指数价格|
|marketPriceInfo|String|现货指数价格-解释|
|fairPrice|String|合理价格|
|fairePriceInfo|String|合理价格-解释|
|price24Max|String|24小时最高价|
|price24Min|String|24小时最低价|
|amount24h|String|24小时交易额|
|lastPriceToCNY|String|最新价To CNY|
|quantity24h|String|24小时交易量|

## 获取Hopex合约深度信息

> Request:

```json
"https://api2.hopex.com/api/v1/depth"
{
  "param": {
    "contractCode": "BTCUSDT"
  }
}
```

`2. Post /api/v1/depth`   访问频率 10次/秒

### 请求参数    

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|

> Response:	

```json
{
  "data": {
    "decimalplace": "1",
    "intervals": [
      "0.5"
    ],
    "asksFilter": "3545.9",
    "asks": [
      {
        "priceD": 3511,
        "orderPrice": "3511.0",
        "orderQuantity": 28701,
        "orderQuantityShow": "28,701",
        "exist": 0
      },
      // ...
    ],
    "bidsFilter": "3475.6",
    "bids": [
      {
        "priceD": 3510.5,
        "orderPrice": "3510.5",
        "orderQuantity": 14619,
        "orderQuantityShow": "14,619",
        "exist": 0
      },
      // ...
    ]
  },
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1548156640871
 }    
```

### 返回值说明

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|decimalplace|String| 精度|
|intervals|String| 区间|
|asksFilter|String| 最大卖价|
|asks|String| 卖盘|
|bidsFilter|String| 最小买价|
|bids|String| 买盘|
|priceD|float| 价格|
|orderPrice|String| 价格|
|orderQuantity|int| 数量|
|exist|String| 此价格是否有用户自己的委托|

## 获取Hopex新成交信息

> Request:

```json
curl "https://api2.hopex.com/api/v1/trades?contractCode=BTCUSDT&pageSize=1"
```

`3. Get /api/v1/trades`   访问频率 10次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|pageSize|int|是|请求个数|

> Response:

```json
{
  "data": [
    {
      "id": 3102886,
      "time": "19:45:20",
      "timestamp": 1573540239.530246,      
      "fillPrice": "3504.0",
      "fillQuantity": "1,603",
      "side": "1"
    }
  ],
  "ret": 0,
  "env": 0,
  "errCode": null,
  "errStr": null,  
  "timestamp": 1548157523524
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|id|long|最新成交id|
|time|String| 成交时间|
|timestamp|float| 成交时间戳|
|fillPrice|String| 成交价格|
|fillQuantity|String| 成交数量|
|side|String| 方向 1 sell 2 buy|

##获取Hopex K线

> Request:

```json
curl "https://api2.hopex.com/api/v1/kline?contractCode=BTCUSDT&endTime=1603881210&startTime=1603814400&interval=1m"
(最多只能获取1000根K线数据)
```

`4. Get /api/v1/kline`   访问频率 10次/秒

### 请求参数    

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|endTime|int64|是|结束时间戳(单位秒)|
|startTime|int64|是|开始时间戳(单位秒)|
|interval|String|是|间隔标识, 1m->1分钟K线 5m->5分钟K线 1h->1小时K线 1d->天K线 1w->周K线 1M->月K线 依此类推|

> Response:

```json
# Request
GET 
# Response
{
    "data": {
        "decimalplace": "1",
        "timeData": [
            {
                "time": 1561223880,
                "open": "10901.0",
                "close": "10876.5",
                "high": "10901.0",
                "low": "10867.5",
                "vol": "194911",
                "val": "424098.559",
                "prevClose": "0.0",
                "upDown": "-24.50",
                "upDownRate": "-0.22%",
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
    "timestamp": 1561343966013
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|data|String| K线数据|
|decimalplace|String| 小数点位数|
|time|long| 时间戳(秒)|
|open|String| 开市值|
|close|String| 闭市值|
|high|String| 最高价|
|low|String| 最低价|
|vol|String| 成交量|
|val|String| 成交额|
|prevClose|String| 上一笔的闭市价|
|upDown|String| 涨跌额|
|upDownRate|String| 涨跌率|
|direct|int| 合约方向|
|contractValue|String| 合约价值|

## 获取Hopex所有合约行情

```json
curl "https://api2.hopex.com/api/v1/markets"
```

`5. Get /api/v1/markets`   访问频率 10次/秒

> Response:	

```json
{
    "data": [
        {
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT永续",
            "allowTrade": true,
            "hasPosition": false,
            "closeCurrency": "USDT",
            "quotedCurrency": "USDT",
            "precision": 2,
            "minPriceMovement": 0.5,
            "pricePrecision": 1,
            "lastestPrice": 3904.5,
            "changePercent24h": -0.00153433064825469888761028,
            "sumAmount24h": 7354.4393639723098773,
            "sumAmount24hUSDT": 75235914.6934367300447790,
            "posVauleUSD": "234,406,032.14"
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

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|contractCode|String| 合约编码|
|contractName|String| 合约名称|
|allowTrade|bool| 是否允许交易|
|hasPosition|bool| 是否有持仓|
|closeCurrency|String| 结算货币|
|quotedCurrency|String| 标价币种|
|precision|int| 合理价格精度|
|minPriceMovement|float| 最小变动价位|
|pricePrecision|int| 价格精度|
|lastestPrice|float| 最新价|
|changePercent24h|float| 24小时涨跌幅|
|sumAmount24h：24小时交易额, 以结算货币为单位|float|undefined|
|sumAmount24hUSDT|float| 24小时交易额, 以USDT为单位,|
|posVauleUSD|String| 合约未平仓量价值,以USD为单位|

## 获取Hopex成交量

> Request: 

```json
curl "https://api2.hopex.com/api/v1/indexStat"
```

`6. Get /api/v1/indexStat`    访问频率 10次/秒

> Response:	

```json
{
  "data": {
        "posVauleUSD": "568,467,422.95",
        "posVauleCNY": "3,943,771,170.06",
        "amount24hUSD": "1,504,196,973.56",
        "amount24hCNY": "10,435,441,713.95",
        "amount7dayUSD": "12,702,202,974.38",
        "amount7dayCNY": "88,122,168,244.92",
        "userCount": "527,670",
        "dealCountUSD": "1,765,475,066,609.42",
        "dealCountCNY": "12,248,071,548,356.19"
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1583300598281
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|posVauleUSD|String| 未平仓合约价值(USD)|
|posVauleCNY|String| 未平仓合约价值(CNY)|
|amount24hUSD|String| 24小时交易额(USD)|
|amount24hCNY|String| 24小时交易额(CNY)|
|amount7dayUSD|String| 7天交易额(USD)|
|amount7dayCNY|String| 7天交易额(CNY)|
|userCount|String| 用户数|
|dealCountUSD|String| 总交易额(USD)|
|dealCountCNY|String| 总交易额(CNY)|

## 获取Hopex交易所推送公告

> Request:

```json
curl "https://api2.hopex.com/api/v1/index_notify?page=1&limit=5&culture=zh-CN"
```

`7. Get /api/v1/index_notify`   访问频率 10次/秒

### 请求参数    

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|page|int|否|第几页,默认1, Request Query|
|limit|int|否|每页条数,默认10, Request Query|
|culture|String|否|语言(zh-CN,en,zh-HK),默认zh-CN,不同语言得到公告数量有可能不一致,以zh-CN为准,Request Query|

> Response:  

```json
{
    "data": {
        "totalCount": 42,
        "page": 1,
        "pageSize": 5,
        "result": [{
            "id": 131,
            "title": "3月10日升级优化完成公告",
            "link": "https://h5.hopex.com/notify/index.html?id=131",
            "lastModifiedTime": "2020-03-10 11:32:09",
            "time": "2020-03-10",
            "timestamp": 1583811129
        }]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1583997209048
}
```

### 返回值说明   

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|totalCount|String| 发布公告总数|
|title|String| 公告标题|
|link|String| 公告跳转链接|
|lastModifiedTime|String| 最新更新时间|
|timestamp|long| 时间戳(秒)|


# 合约交易 API

## 获取Hopex用户信息

> Request:

```json
curl "https://api2.hopex.com/api/v1/userinfo"
```

`1. Get /api/v1/userinfo`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|

> Response:	

```json
{
    "data": {
        "profitRate": "0.00%",
        "totalWealth": "0.00",
        "totalWealthUSD":"0.00",
        "floatProfit": "0.00",
        "floatProfitUSD": "0.00",
        "position": 0,
        "activeOrder": 0
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555554824314
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|profitRate|String| 当前持仓收益率(浮动盈亏/持仓占用保证金)|
|totalWealth|String| 账户总权益估值(BTC)|
|totalWealthUSD|String| 账户总权益估值(USD)|
|floatProfit|String| 总浮动盈亏估值(BTC)|
|floatProfitUSD|String| 总浮动盈亏估值(USD)|
|position|int| 持仓数|
|activeOrder|int| 活跃委托数|

## 用户下单

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

`2. Post /api/v1/order`   访问频率 1次/秒

### 请求参数
|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|side|int|是|1:买入开多, 2:卖出开空, 3:买入平空, 4:卖出平多|
|orderQuantity|int|是|订单数量|
|orderPrice|number|否|最小变动价位整数倍,不填时表示下市价单|

> Response:	

```json
{
    "data": 1950354706,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555559102763
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|ret|int| 为0表示下委托成功|
|data|long| 订单id|

## 用户撤单

> Request:

```json
curl "https://api2.hopex.com/api/v1/cancel_order?orderId=1952293255&contractCode=BTCUSDT"
```

`3. Get /api/v1/cancel_order`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|orderId|int|是|委托id|
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|

> Response:	

```json
{
    "data": true,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555569296445
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|ret|int| 为0且data为true时表示撤单成功|
|data|bool| true表示撤单成功| 


## 获取用户的活跃委托

> Request:

```json
curl "https://api2.hopex.com/api/v1/order_info?contractCode=BTCUSDT"
```

`4. Get /api/v1/order_info`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCode|String|否|不传时查所有合约的活跃委托|

> Response:	

```json
{
    "data": [
        {
            "orderId": 120396444,
            "orderType": "买入开多",
            "orderTypeVal": 1,
            "direct": 1,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT永续",
            "type": "1",
            "side": "2",
            "sideDisplay": "买入",
            "ctime": "2019-06-26 15:03:09",
            "mtime": "2019-06-26 15:03:09",
            "orderQuantity": "+1,000",
            "leftQuantity": "1,000",
            "fillQuantity": "0",
            "orderStatus": "2",
            "orderStatusDisplay": "等待成交",
            "orderPrice": "12785.5",
            "leverage": "20.00",
            "fee": "--",
            "avgFillMoney": "--",
            "orderMargin": "66.4846 USDT",
            "expireTime": "2019-07-03 15:03:09"
        }
    ],
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1561532593797
}   
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|orderId|long| 订单ID|
|orderType|String| 委托类型 1:买入开多, 2:卖出开空, 3:买入平空, 4:卖出平多|
|orderTypeVal|int| 委托类型 1:买入开多, 2:卖出开空, 3:买入平空, 4:卖出平多|
|direct|int| 1:多仓，2:空仓|
|contractCode|String| 合约code|
|contractName|String| 合约name|
|type|String| 1.限价开仓 2.市价开仓 3.限价全平 4.市价全平 5.限价部分平仓单 6.市价部分平仓单|
|side|String| 方向 1:卖出 2:买入|
|sideDisplay|String| 方向 1:卖出 2:买入|
|ctime|String| 创建时间|
|mtime|String| 更新时间|
|orderQuantity|String| 数量（张）|
|leftQuantity|String| 还剩下多少没有成交|
|fillQuantity|String| 已经成交的数量|
|orderStatus|String| 订单状态|
|orderStatusDisplay|String| 订单状态|
|orderPrice|String| 委托价|
|leverage|String| 杠杆倍数（2位小数）|
|fee|String| 手续费(小数点后4位)|
|avgFillMoney|String| 成交均价(指数_合理价格小数位数)|
|orderMargin|String| 委托保证金(小数点后4位)|
|expireTime|String| 过期时间|


## 获取历史委托

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

`5. Post /api/v1/order_history`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCodeList|String[]|是|合约列表,为空查询所有, Request Body|
|typeList|int[]|是|1.限价开仓 2.市价开仓 3.限价全平 4.市价全平 5.限价部分平仓单 6.市价部分平仓单, Request Body|
|side|int|是|0:no limit 1 for sell, 2 for buy, Request Body|
|startTime|int|是|开始时间戳, Request Body|
|endTime|int|是|结束时间戳, Request Body|
|page|int|是|第几页,默认1, Request Query|
|limit|int|否|每页条数,默认10, Request Query|

> Response:	

```json
{
    "data": {
        "totalCount": 0,
        "page": 2,
        "pageSize": 10,
        "result": [
            {
                "orderId": 1935768402,
                "contractCode": "BTCUSDT",
                "contractName": "BTC/USDT永续",
                "type": "4",
                "side": "1",
                "direct": 2,
                "sideDisplay": "卖出",
                "ctime": "2019-04-17 10:55:51",
                "ftime": "2019-04-17 10:55:51",
                "orderQuantity": "-100",
                "fillQuantity": "-100",
                "orderStatus": "2",
                "orderStatusDisplay": "完全成交",
                "orderPrice": "市价",
                "leverage": "20.00",
                "fee": "0.0522 USDT",
                "avgFillMoney": "5219.50",
                "closePosPNL": "-0.0050 USDT",
                "timestamp": 1555469751974534,
                "orderTypeVal" : 2,
                "orderType": "卖出开空",
                "cancelReason": "撤销原因"
            },
            //...
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555571374944
}      
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|orderId|long| 订单ID|
|contractCode|String| 合约code|
|contractName|String| 合约name|
|type|String| 1.限价 2.市价 3.限价全平 4.市价全平|
|side|String| 方向 1.卖出 2.买入|
|direct|int| 订单对应的持仓方向 1:多仓，2:空仓|
|sideDisplay|String| 方向|
|ctime|String| 创建时间|
|ftime|String| 完成时间|
|orderQuantity|String| 数量（张）|
|fillQuantity|String| 已经成交的数量|
|orderStatus|String| 订单状态|
|orderStatusDisplay|String| 订单状态|
|orderPrice|String| 委托价|
|leverage|String| 杠杆倍数（2位小数）|
|fee|String| 手续费(小数点后4位)|
|avgFillMoney|String| 成交均价(指数_合理价格小数位数)|
|closePosPNL|String| 平仓盈亏 小数点后4位|
|timestamp|long| 时间戳(微秒)|
|orderTypeVal|String| 1|
|orderType|String| 买入开多 卖出开空 买入平空 卖出平多|
|cancelReason|String| 撤销原因|


## 获取持仓

> Request:

```json
curl "https://api2.hopex.com/api/v1/position"
```

`6. Get /api/v1/position`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCode|String|否|合约列表|

> Response:	

```json
{
    "data": [
        {
            "allowFullClose": true,
            "contractCode": "BTCUSDT",
            "contractName": "BTC/USDT永续",
            "leverage": "20.00",
            "contractValue": "0.0001",
            "maintMarginRate": "0.005",
            "takerFee": "0.001",
            "positionQuantity": "+100",
            "direct": 2,
            "posiDirect": 1,
            "posiDirectD": "持多",
            "entryPrice": "5261.00",
            "entryPriceD": 5261,
            "positionMargin": "2.6831 USDT",
            "positionMarginD": 2.68311,
            "liquidationPrice": "5024.02",
            "maintMargin": "0.3133 USDT",
            "unrealisedPnl": "-0.0025 USDT",
            "unrealisedPnlPcnt": "-0.09%",
            "fairPrice": "5260.97",
            "fairPriceD": 5260.97,
            "lastPrice": "5260.5",
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

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|allowFullClose|bool| 允许全平|
|contractCode|String| 合约编码|
|contractName|String| 合约名称|
|leverage|String| 杠杆倍数|
|contractValue|String| 合约价值|
|maintMarginRate|String| 合约的维持保证金率|
|takerFee|String| 提取方费率|
|positionQuantity|String| 持仓量|
|direct|int| 持仓方向 1:多仓，2:空仓|
|posiDirect|int| 持仓方向（持多 1 /持空 -1)|
|posiDirectD|String| 持仓方向（持多/持空)|
|entryPrice|String| 开仓均价|
|entryPriceD|float| 开仓均价|
|positionMargin|String| 持仓占用保证金|
|positionMarginD|float| 持仓占用保证金|
|liquidationPrice|String| 强平价格|
|maintMargin|String| 维持保证金|
|unrealisedPnl|String| 浮动盈亏|
|unrealisedPnlPcnt|String| 收益率|
|fairPrice|String| 合理价格|
|fairPriceD|float| 合理价格|
|lastPrice|String| 最新成交价|
|sequence|int| 档长度|
|rank|int| 用户落在第几档|
|minPriceMovement|float| 最小变动价位|
|minPriceMovementPrecision|int| 最小变动价位精度|
|positionQuantityFreeze|String| 冻结的持仓量,等待成交的平仓订单的总量|
|closeablePositionQuantity|String| 可平数量|
|isAddMargin |bool| 是否设置追加保证金|
|sort|int| 排序|
|isWhitelistExist|bool| 是否存在白名单|
|closeCurrency|String| 结算货币|


## 用户下条件单

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

`7. Post /api/v1/condition_order`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|side|int|是|1:买入开多, 2:卖出开空, 3:买入平空, 4:卖出平多|
|type|String|是|Limit:限价单 Market:市价单|
|trigPrice|number|是|触发价格|
|expectedQuantity|int|是|预设数量|
|expectedPrice|number|是|预设价格|

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

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|ret|int| 为0且data为true时表示下计划单成功|
|data|bool| true表示下计划单成功|

## 查询条件单列表

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

`8. Post /api/v1/condition_order_info`    ,访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCodeList|String[]|否|合约列表,为空查询所有, Request Body|
|taskTypeList|int[]|否|条件单类别 1.买入开多, 2.卖出开空, 3.买入平空, 4.卖出平多,为空查所有, Request Body|
|trigTypeList|int[]|否|条件单触发类别  1:市场价 2:合理价格 为空查所有, Request Body|
|taskStatusList|int[]|否|条件单状态 1:已创建 2.已撤销 3.已触发委托成功 4.已触发委托失败,为空查所有, Request Body|
|direct|int|是|1:多仓 2:空仓,0:查询所有|
|side|int|是|方向1:sell 2:buy,0:查询所有|
|startTime|number|是|0:查询所有,开始时间戳(单位微秒)|
|endTime|number|是|0:查询所有,结束时间戳(单位微秒)|

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
                "taskTypeD": "买入开多",
                "taskId": 207565,
                "contractCode": "BTCUSDT",
                "contractName": "BTC/USDT永续",
                "action": 0,
                "direct": 1,
                "side": 2,
                "taskStatus": 1,
                "taskStatusD": "未触发",
                "trigType": 1,
                "trigTypeD": "市场价>=14000.00",
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
                "failureReason": "创建成功",
                "leverage": "30.00"
            },
			//...
            {
                "taskType": 1,
                "taskTypeD": "买入开多",
                "taskId": 207564,
                "contractCode": "BTCUSDT",
                "contractName": "BTC/USDT永续",
                "action": 0,
                "direct": 1,
                "side": 2,
                "taskStatus": 1,
                "taskStatusD": "未触发",
                "trigType": 1,
                "trigTypeD": "市场价<=12000.00",
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
                "failureReason": "创建成功",
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

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|taskType|int|条件单类型,1.买入开多, 2.卖出开空, 3.买入平空, 4.卖出平多|
|taskId|long|条件单任务id|
|contractCode|String|合约编码|
|contractName|String|合约名称|
|action|int|1.开仓 2.平仓|
|direct|int|1.多仓 2.空仓|
|side|int|1.卖 2.买|
|taskStatus|int|条件单任务当前状态，1:创建ok 2.已撤销 3.已触发委托成功 4.已触发委托失败|
|trigType|int|1.市场价 2.合理价格|
|trigPrice|String|触发价格|
|expectedQuantity|String|期待数量|
|expectedPrice|String|期待价格|
|expireTime|String|有效期|
|orderId|long|委托ID|
|orderQuantity|String|订单里面的委托数量|
|orderPrice|String|订单里面的委托价格|
|leverage|String|杠杆倍数|


## 用户撤条件单

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

`9. Post /api/v1/cancel_condition_order`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|
|taskId|int|是|条件单ID|

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

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|ret|int| 为0且data为true时表示撤条件单成功|
|data|bool| true表示撤条件单成功|



## 获取资产信息

> Request:

```json
curl "https://api2.hopex.com/api/v1/wallet"
```

`10. Get /api/v1/wallet`    访问频率 1次/秒

### 请求参数
|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|

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
                "totalWealthInfo": "总权益 = 钱包余额 + 浮动盈亏",
                "availableBalance": "0.77484596",
                "availableBalanceLegal": "0.78 USD",
                "availableBalanceInfo": "可用余额 = 钱包余额 - 持仓占用保证金 - 委托占用保证金 - 出金冻结金额",
                "walletBalance": "0.77484596",
                "walletBalanceLegal": "0.78 USD",
                "walletBalanceInfo": "钱包余额 = 入金 - 出金 + 平仓盈亏 - 交易手续费",
                "positionMargin": "0.00000000",
                "positionMarginLegal": "0.00 USD",
                "delegateMargin": "0.00000000",
                "delegateMarginLegal": "0.00 USD",
                "withdrawFreeze": "0.00000000",
                "withdrawFreezeLegal": "0.00 USD",
                "depositAmount": "1266.87475194",
                "depositAmountLegal": "1267.95 USD",
                "withdrawAmount": "1231.50005415",
                "withdrawAmountLegal": "1232.55 USD"
            },
            {
                "assetName": "BTC",
                "assetLogoUrl": null,
                "floatProfit": "0.00000000",
                "floatProfitLegal": "0.00 USD",
                "profitRate": "0.00% ",
                "totalWealth": "0.00000000",
                "totalWealthLegal": "0.00 USD",
                "totalWealthInfo": "总权益 = 钱包余额 + 浮动盈亏",
                "availableBalance": "0.00000000",
                "availableBalanceLegal": "0.00 USD",
                "availableBalanceInfo": "可用余额 = 钱包余额 - 持仓占用保证金 - 委托占用保证金 - 出金冻结金额",
                "walletBalance": "0.00000000",
                "walletBalanceLegal": "0.00 USD",
                "walletBalanceInfo": "钱包余额 = 入金 - 出金 + 平仓盈亏 - 交易手续费",
                "positionMargin": "0.00000000",
                "positionMarginLegal": "0.00 USD",
                "delegateMargin": "0.00000000",
                "delegateMarginLegal": "0.00 USD",
                "withdrawFreeze": "0.00000000",
                "withdrawFreezeLegal": "0.00 USD",
                "depositAmount": "0.00085151",
                "depositAmountLegal": "5.90 USD",
                "withdrawAmount": "0.00085151",
                "withdrawAmountLegal": "5.90 USD"
            },
            {
                "assetName": "ETH",
                "assetLogoUrl": null,
                "floatProfit": "0.00000000",
                "floatProfitLegal": "0.00 USD",
                "profitRate": "0.00% ",
                "totalWealth": "0.00000000",
                "totalWealthLegal": "0.00 USD",
                "totalWealthInfo": "总权益 = 钱包余额 + 浮动盈亏",
                "availableBalance": "0.00000000",
                "availableBalanceLegal": "0.00 USD",
                "availableBalanceInfo": "可用余额 = 钱包余额 - 持仓占用保证金 - 委托占用保证金 - 出金冻结金额",
                "walletBalance": "0.00000000",
                "walletBalanceLegal": "0.00 USD",
                "walletBalanceInfo": "钱包余额 = 入金 - 出金 + 平仓盈亏 - 交易手续费",
                "positionMargin": "0.00000000",
                "positionMarginLegal": "0.00 USD",
                "delegateMargin": "0.00000000",
                "delegateMarginLegal": "0.00 USD",
                "withdrawFreeze": "0.00000000",
                "withdrawFreezeLegal": "0.00 USD",
                "depositAmount": "0.07003782",
                "depositAmountLegal": "11.03 USD",
                "withdrawAmount": "0.07003782",
                "withdrawAmountLegal": "11.03 USD"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1555583462806
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|conversionCurrency|String| 计价货币|
|totalWealth|String| 总权益|
|totalWealthLegal|String| 总权益 (USD)|
|floatProfit|String| 浮动盈亏|
|floatProfitLegal|String| 浮动盈亏 (USD)|
|availableBalance|String| 可用余额|
|availableBalanceLegal|String| 可用余额 (USD)|
|assetName|String| 货币名称|
|assetLogoUrl|String| 货币logo|
|profitRate|String| 收益率|
|walletBalance|String| 钱包余额|
|walletBalanceLegal|String| 钱包余额 (USD)|
|positionMargin|String| 持仓保证金|
|positionMarginLegal|String| 持仓保证金 (USD)|
|delegateMargin|String| 委托占用保证金|
|delegateMarginLegal|String| 委托占用保证金 (USD)|
|withdrawFreeze|String| 提现冻结保证金|
|withdrawFreezeLegal|String| 提现冻结保证金 (USD)|
|depositAmount|String| 入金|
|depositAmountLegal|String| 入金(USD)|
|withdrawAmount|String| 出金|
|withdrawAmountLegal|String| 出金 (USD)|

## 设置用户合约杠杆

> Request:

```json
curl "https://api2.hopex.com/api/v1/set_leverage?contractCode=BTCUSDT&direct=2&leverage=70"
```

`11. Get /api/v1/set_leverage`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCode|String|是|合约名字|
|direct|int|是|方向:1 多仓，2 空仓|
|leverage|number|是|杠杆倍数|

> Response:	

```json
{
    "data": 70.00,
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1563010363110
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|data|float| 设置成功的杠杆倍数(2位小数)|


## 获取下单参数

> Request:

```json
"https://api2.hopex.com/api/v1/get_orderParas"
{
  "param": {
    "contractCode": "BTCUSDT"
  }
}
```

`12. Post /api/v1/get_orderParas`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCode|String|是|BTCUSDT ETHUSDT BTCUSD ETHUSD等|

> Response:  

```json
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
    "minTradeNumDisplay": "1张",
    "availableBalance": null,
    "availableBalanceDisplay": null,
    "maxBuyPrice": "7025.5",
    "minSellPrice": "6616.0",
    "longMinLeverage": "1.00",
    "longMaxLeverage": "100.00",
    "shortMinLeverage": "1.00",
    "shortMaxLeverage": "100.00",
    "longDefaultLeverage": "100.00",
    "shortDefaultLeverage": "100.00",
    "longLeverage": "100.00",
    "shortLeverage": "100.00",
    "openLongMargin": null,
    "openLongMarginDisplay": null,
    "openShortMargin": null,
    "openShortMarginDisplay": null,
    "openLongAmount": 0,
    "openShortAmount": 0,
    "closeLongAmount": 0,
    "closeShortAmount": 0,
    "evaluateOrderValue": "0.0000 BTC",
    "precision": 2
  },
  "ret": 0,
  "errCode": null,
  "errStr": null,
  "env": 0,
  "timestamp": 1586778167221
}
```

### 返回值说明   

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|longMinLeverage|String| 多仓杠杆最小值|
|longMaxLeverage|String| 多仓杠杆最大值|
|shortMinLeverage|String| 空仓杠杆最小值|
|shortMaxLeverage|String| 空仓杠杆最大值|

## 获取强平历史

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

`13. Post /api/v1/liquidation_history`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|contractCodeList|String[]|是|合约列表,为空查询所有|
|side|int|是|0:no limit 1 for sell, 2 for buy.|
|page|int|是|第几页,默认1|

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
                "contractName": "ETH/USDT永续",
                "side": "2",
                "sideDisplay": "买入",
                "orderType": "买入平空",
                "orderTypeVal": 3,
                "direct": 2,
                "leverage": "20.00",
                "orderQuantity": "+70,731",
                "orderPrice": "194.43",
                "closePosPNL": "-6545.5086 USDT",
                "fee": "68.7606 USDT",
                "ctime": "2019-10-30 14:19:52",
                "timestamp": 1572416393251656,
                "direction": 1,
                "directionDisplay": "空",
                "positionMargin": "+6614.2692 USDT",
                "openPrice": "185.17",
                "liquidationPriceReal": "193.50",
                "showDetail": false
            },
	    	//...
            {
                "orderId": 2,
                "contractCode": "EOSUSDT",
                "contractName": "EOS/USDT永续",
                "side": "1",
                "sideDisplay": "卖出",
                "orderType": "卖出平多",
                "orderTypeVal": 4,
                "direct": 1,
                "leverage": "20.00",
                "orderQuantity": "-1,123",
                "orderPrice": "6.4883",
                "closePosPNL": "-383.6963 USDT",
                "fee": "3.6432 USDT",
                "ctime": "2019-06-27 04:44:47",
                "timestamp": 1561581887104635,
                "direction": 2,
                "directionDisplay": "多",
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
    "timestamp": 1572501526044
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|orderId|long| 订单ID|
|contractCode|String| 合约code|
|contractName|String| 合约name|
|side|int| 方向 1.卖出 2。买入|
|sideDisplay|String| 方向|
|orderType|String| 1.买入开多 2.卖出开空 3.买入平空 4.卖出平多|
|orderTypeVal|int| 1.买入开多 2.卖出开空 3.买入平空 4.卖出平多|
|direct|int| 订单对应的持仓方向 1.多仓 2.空仓|
|leverage|String| 杠杆倍数（2位小数）|
|orderQuantity|String| 数量（张）|
|orderPrice|String| 价格|
|closePosPNL|String| 平仓盈亏 小数点后4位|
|fee|String| 手续费(小数点后4位)|
|ctime|String| 创建时间|
|timestamp|long| 时间戳(微秒)|
|direction|int| 1.空 2.多|
|positionMargin|String| 持仓占用保证金 (4位小数)|
|openPrice|String| 开仓均价|
|liquidationPriceReal|String| 强平价格|
|showDetail|bool| 忽略| 

## 获取用户出入金记录

> Request:

```json
curl "https://api2.hopex.com/api/v1/account_records?page=1&limit=10"
```

`14. Get /api/v1/account_records`    访问频率 1次/秒

### 请求参数

|参数名|   参数类型|   必填| 描述|
| :-----    | :-----   | :-----    | :-----   |
|Authorization|String|是|用户信息验证, Request Header|
|Date|String|是|当前的GMT时间, Request Header|
|Digest|String|是|请求包体摘要, Request Header|
|page|int|否|第几页,默认1|
|limit|int|否|每页返回条数,默认20|

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
                "orderTypeD": "普通链上出金",
                "amount": "-1900.00000000 USDT",
                "rmbAmount": "0.00 CNY",
                "bankName": "",
                "addr": "mvYW73ZTo8F7aksBrWad3XvqViqDE3saaP",
                "orderStatus": 1,
                "orderStatusD": "完成",
                "createdTime": "2019-10-31 15:52:11"
            },
            //...
            {
            	"id": 58421,
                "asset": "USDT",
                "orderType": 2,
                "orderTypeD": "OTC出金",
                "amount": "-100.00000000 USDT",
                "rmbAmount": "704.00 CNY",
                "bankName": "邮政",
                "addr": "",
                "orderStatus": 1,
                "orderStatusD": "完成",
                "createdTime": "2019-07-27 15:02:21"
            }
        ]
    },
    "ret": 0,
    "errCode": null,
    "errStr": null,
    "env": 0,
    "timestamp": 1572935091833
}
```

### 返回值说明	

|参数名| 参数类型| 描述|
| :-----    | :-----   | :-----    |
|asset|String| 币种|
|orderType|int| 1 OTC入金，2 OTC出金，3 链上入金，4 链上出金，5 内部转账-入金，6 内部转账-出金, 7 人工入金, 8 人工出金,9 快速入金,10 快速出金|
|orderTypeD|String| 交易类型描述|
|amount|String| 数字货币金额|
|rmbAmount|String| 人民币金额|
|bankName|String| 银行名|
|addr|String| 钱包地址|
|orderStatus|int| 0 进行中，1 完成，2失败|
|orderStatusD|String| 交易状态描述|
|createdTime|String| 时间| 

# API验签

## 验证细节

验证的细节分为以下四个方面:

### 生成API Key

对任何请求进行签名之前,须通过Hopex创建属于您的API Key和API Secret, API Key和API Secret将由Hopex随机生成和提供.

### 发起请求

所有私有REST请求头都必须包含Authorization, Date和Digest.

### 签名

Authorization头的格式如下: `-H 'Authorization: hmac apikey="..."`, `algorithm="hmac-sha256"`, `headers="date request-line digest"`, `signature="..."'`, apikey为您的`API Key`, signature为`method+request-line+" HTTP/1.1"+digest`, 用secret使用`HMAC SHA256`方法加密，通过BASE64编码输出而得到的, `request-line`是请求接口路径. 如: `/api/v1/userinfo`, digest是请求body的摘要.

### GMT时间

Date请求头必须是发送请求时刻对应的标准格林威治时间格式,如 Fri, 19 Apr 2019 03:15:20 GMT, 如果发送请求时刻和服务器时间相差1分钟以上的请求将被系统视为过期并拒绝.

## 示例程序

右边是生成Authorization, Date和Digest Request Header的示例程序:

> Javascript

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

## 示例Demo

以下是使用工具Postman,通过Post请求来获取历史委托的演示Demo:

![Hopex图片](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/10-250-637393902050348151.png)

![Hopex图片](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/11-223-637393902082612713.png)

![Hopex图片](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/12-245-637393902111464004.png)

![Hopex图片](https://hkhposs.oss-accelerate.aliyuncs.com/fbanners/13-168-637393902133510542.png)



