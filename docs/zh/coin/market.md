---
sort: 4 # follow a certain sequence of letters or numbers
---
# 行情数据

&nbsp;

### 实时ticker数据

- GET /v1/market/ticker

```json
https://hkapi.hotcoin.top/v1/market/ticker

curl "https://hkapi.hotcoin.top/v1/market/ticker"
```

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
status|y|string|状态码||成功：ok，失败：error
timestamp|y|long|当前毫秒数||
ticker|y|list|数据||

**响应数据:**

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

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
symbol|y|string|交易对symbol||卖币种简称_买币种简称简称，eg：btc_usdt
last|y|number|最新价||
buy|y|number|买一价||
sell|y|number|卖一价||
high|y|number|24小时最高价 ||
low|y|number|24小时最低价||
vol|y|number|24小时成交量||
change|y|number|24小时涨跌幅||

&nbsp;

### 获取k线数据

-  GET /v1/ticker

```json
https://hkapi.hotcoin.top/v1/ticker?symbol=btc_usdt&step=60

curl "https://hkapi.hotcoin.top/v1/ticker?symbol=btc_usdt&step=60"
```


**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
step|y|int|时间：秒||60（1分钟）,300（5分钟）,900（15分钟）,1800（30分钟）,3600（1小时）,86400（1天）,604800（1周）,2592000（1月）
symbol|y|string|交易对||例：btc_gavc

**响应数据:**

```json
{
  "code":200,
  "msg":"成功",
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

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
time|y|long|当前毫秒数
data|y|array(array(number))|K线数据

**data:**<br>

[[ <br>
1527820200000,   //int 时间<br>
54598.5,         //number  开<br>
54598.5,         //number  高<br>
54598.5,         //number  低<br>
54598.5,         //number  收<br>
0.0000          //number  量<br>
],<br>
......<br>
]<br>

&nbsp;

### 获取深度数据


- GET /v1/depth

```json
https://hkapi.hotcoin.top/v1/depth?symbol=btc_usdt&step=60

curl "https://hkapi.hotcoin.top/v1/depth?symbol=btc_usdt&step=60"
```



**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
symbol|y|string|交易对||例：btc_gavc
step|n|int|加上此参数可查最新一个k线数据，类型为时间，单位秒||60,3*60,5*60,15*60,30*60,60*60（1小时）,24*60*60（1天）,7*24*60*60（1周）,30*24*60*60（1月）


**响应数据:**

```json
{
  "code":200,
  "msg":"成功",
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

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
time|y|long|当前毫秒数
data|y|object|交易深度数据

**data:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
depth|y|object
period|n|object|传step时才有值

**depth:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
bids|y|array(array(long))|买盘,[price(成交价), amount(成交量)]
asks|y|array(array(long))|卖盘,[price(成交价), amount(成交量)]
date|y|long|时间戳
lastPrice|y|number|最新成交价

**period:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
marketFrom|y|string|入参symbol
coinVol|y|string|入参symbol
type|y|long|入参step,时间
data|y|array（array）|最后一个k线数据，格式同上，但只有一个

&nbsp;

### 获取实时成交数据


- GET /v1/trade

```json
https://hkapi.hotcoin.top/v1/trade?symbol=btc_usdt&count=60

curl "https://hkapi.hotcoin.top/v1/trade?symbol=btc_usdt&count=60"
```

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
count|y|int|Trades条数||0
symbol|y|string|交易对||例：btc_gavc

**响应数据:**

```json
{
  "code":200,
  "msg":"成功",
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
        "type":"卖出"
      },
      {
        "price":0.007,
        "amount":66491.04,
        "id":1,
        "time":"02:45:08",
        "en_type":"ask",
        "type":"卖出"
      }
    ]
  }
}


```

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
time|y|long|当前毫秒数
data|y|object|实时成交数据

**data:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
trades|y|array(object)|trades数据
sellSymbol|y|string|sellSymbol
buySymbol|y|string|buySymbol

**trades:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
price|y|long|成交价钱
amount|y|string|成交数量
id|y|string|成交id
time|y|string|成交时间
en_type|y|string|成交方向||"bid"(买入),"ask"(卖出)
type|y|string|成交类型||"买入","卖出"

