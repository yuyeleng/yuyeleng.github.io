---
sort: 3 # follow a certain sequence of letters or numbers
---
# Market Interface

&nbsp;

### Real-time Ticker data

- GET /v1/market/ticker



**Request parameters:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
status|y|string|Status code||success：ok，failed：error
timestamp|y|long|Current millisseconds||
ticker|y|list|data||

**Response data:**

```json
{
  "status":"ok",
  "timestamp":1567045034,
  "ticker":[
    {
      "symbol":"btc_usdt",
      "last":"10000.00000000",
      "buy":"9999.00000000",
      "sell":"10001.00000000",
      "high":"11000.00000000",
      "low":"9000.00000000",
      "vol":"10000000.0000",
      "change":"10.10"
    }
  ]
}
```

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
symbol|y|string|Trading pair||sellShortName_buyShortName，eg：btc_usdt
last|y|number|latest price||
buy|y|number|buy||
sell|y|number|sell||
high|y|number|high ||
low|y|number|low||
vol|y|number|vol||
change|y|number|change||

&nbsp;

### 获取k线数据

-  GET /v1/ticker

**Request parameters:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key
SignatureVersion|y|string|Version
SignatureMethod|y|string|Signature Method||HmacSHA256
Signature|y|string|ApiSecret
Timestamp|y|string|Timestamp
step|y|int|Time：Sec||60（1min）,300（5mins）,900（15mins）,1800（30mins）,3600（1h）,86400（1d）,604800（1w）,2592000（1mon）
symbol|y|string|pairs||btc_gavc

**Response data:**

```json
{
  "code":200,
  "msg":"success",
  "time":1527838104874,
  "data":[
    [
      1527820200000,
      54598.5,
      54598.5,
      54598.5,
      54598.5,
      0
    ],
    [
      1527820200000,
      54598.5,
      54598.5,
      54598.5,
      54598.5,
      0
    ]
  ]
}
```

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|return message
time|y|long|Current millisseconds
data|y|array(array(number))|Kline data

**data:**<br>

[[ <br>
1527820200000,   //int time<br>
54598.5,         //number  O<br>
54598.5,         //number  H<br>
54598.5,         //number  L<br>
54598.5,         //number  C<br>
0.0000          //number  Vol<br>
],<br>
......<br>
]<br>

&nbsp;

### Obtain Deep Data：


- GET /v1/depth

**Request parameters:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
symbol|y|string|Trading pair||Example：btc_gavc
step|n|int|Add the Parameter to check latest Kline data,Data Type is time,unit is sec.||60,3*60,5*60,15*60,30*60,60*60（1h）,24*60*60（1d）,7*24*60*60（1w）,30*24*60*60（1mon）


**Response data:**

```json
{
  "code":200,
  "msg":"success",
  "time":1527837164605,
  "data":{
    "period":{
      "data":[
        [
          1527837120000,
          54598.5,
          54598.5,
          54598.5,
          54598.5,
          0
        ]
      ],
      "marketFrom":"btc_gavc",
      "type":60,
      "coinVol":"btc_gavc"
    },
    "depth":{
      "date":1527837163,
      "asks":[
        [
          57373.8,
          0.0387
        ],
        [
          57751.26,
          0.0128
        ],
        [
          57751.26,
          0.0128
        ]
      ],
      "bids":[
        [
          57373.8,
          0.0387
        ],
        [
          57751.26,
          0.0128
        ],
        [
          57751.26,
          0.0128
        ]
      ],
      "lastPrice":54598.5
    }
  }
}
```

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|return message
time|y|long|Current millisseconds
data|y|object|Trading deep date

**data:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
depth|y|object
period|n|object|Vaule only displayed when uploading step

**depth:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
bids|y|array(array(long))|buy,[price(transaction price), amount(transaction vol)]
asks|y|array(array(long))|sell,[price(transaction price), amount(transaction vol)]
date|y|long|Timestamp
lastPrice|y|number|Latest price

**period:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
marketFrom|y|string|Input symbol
coinVol|y|string|Input symbol
type|y|long|Input step,time
data|y|array（array）|Last kline data,same with format above by only one

&nbsp;

### Obtain real-time data


- GET /v1/trade

**Request parameters:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key
SignatureVersion|y|string|Version
SignatureMethod|y|string|Signature Method||HmacSHA256
Signature|y|string|ApiSecret
Timestamp|y|string|Timestamp
count|y|int|Trades items||0
symbol|y|string|Trading pair||example：btc_gavc

**Response data:**

```json
{
  "code":200,
  "msg":"success",
  "time":1536315868962,
  "data":{
    "sellSymbol":"BTC",
    "buySymbol":"GAVC",
    "trades":[
      {
        "price":0.007,
        "amount":66491.04,
        "id":1,
        "time":"02:45:08",
        "en_type":"ask",
        "type":"sell"
      },
      {
        "price":0.007,
        "amount":66491.04,
        "id":1,
        "time":"02:45:08",
        "en_type":"ask",
        "type":"sell"
      }
    ]
  }
}


```

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|return message
time|y|long|Current millisseconds
data|y|object|Real-time transactions

**data:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
trades|y|array(object)|trades data
sellSymbol|y|string|sellSymbol
buySymbol|y|string|buySymbol

**trades:**

Parameter|Mandatory| Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
price|y|long|transaction price
amount|y|string|transaction amount
id|y|string|transaction id
time|y|string|transaction time
en_type|y|string|direction||"bid"(buy),"ask"(sell)
type|y|string|transaction Data Type||"buy","sell"

