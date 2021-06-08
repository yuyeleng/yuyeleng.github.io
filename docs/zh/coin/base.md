---
sort: 1 # follow a certain sequence of letters or numbers
---
# 基础信息
### 获取所有交易对

```json
https://hkapi.hotcoin.top/v1/common/symbols

curl "https://hkapi.hotcoin.top/v1/common/symbols"
```

**HTTP 请求**

- GET /v1/common/symbols

**请求参数：**


**返回字段：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||成功：200
msg|y|string|消息||
time|y|long|当前毫秒数||
data|y|array|symbols列表||


**data结构**

字段类型|数据类型|描述|description（英文）
------------- | ------------- |  ------------- | ------------
baseCurrency|string|交易中的基础币种|baseCurrency code
quoteCurrency|string|交易中的报价币种|quoteCurrency code
pricePrecision|integer|交易对报价的精度（小数点后位数）|price precision
amountPrecision|integer|交易对基础币种计数精度（小数点)|quantity precision
symbolPartition|string|交易区，可能值: [main,innovation]|symbol Partition, example:[main,innovation]
symbol|string|交易对|trade pair code
state|string|交易对状态 enable - 正常；disable-禁用|trade pair status [enable,disable]
minOrderCount|decimal|交易对最小下单量 (下单量指当订单类型为限价单时，下单接口传的'tradeAmount')|min order count
maxOrderCount|decimal|交易对最大下单量 (下单量指当订单类型为限价单时，下单接口传的'tradeAmount')|max order count
minOrderPrice|decimal|最小下单价格（下单金额指当订单类型为限价单时，下单接口传入的‘price’）|min order price
maxOrderPrice|decimal|最大下单价格（下单金额指当订单类型为限价单时，下单接口传入的‘price’）|max order price

返回json示例

```json
{
   "code": "200",
   "time": 1567045034,
   "data": [
   {"baseCurrency":"etc",
    "quoteCurrency":"usdt",
    "pricePrecision":6,
    "amountPrecision":4,
    "symbolPartition":"main",
    "symbol":"etc_usdt",
    "state":"online",
    "minOrderCount":0.001,
    "maxOrderCount":10000,
    "minOrderPrice":0.0001,
    "maxOrderPrice":10000
    },
    {
    "baseCurrency":"ltc",
    "quoteCurrency":"usdt",
    "pricePrecision":6,
    "amountPrecision":4,
    "symbolPartition":"innovation",
    "symbol":"ltc_usdt",
    "state":"online",
    "minOrderCount":0.001,
    "maxOrderCount":10000,
    "minOrderPrice":0.0001,
    "maxOrderPrice":10000
    }
  ]
}
```

### 下单

**HTTP 请求**

- POST /v1/order/place

**请求参数：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
symbol|y|string|交易对| |例：btc_usdt
type|y|string|类型| | "buy" ,”sell"
tradeAmount|y|number|数量||
tradePrice|y|number	|价钱||


**返回字段：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||成功：200，失败：300
msg|y|string|消息||
time|y|long|当前毫秒数||
data|y|object|数据||


msg 范围

中文 | English |
------------ | ------------ 
非法请求 |Illegal request 
请使用正确的数量| Illegal tradeAmount value 
请使用正确的价格| Illegal tradePrice value 
币种ID错误| Illegal symbol format

data

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
ID|y|bigint|订单id||


**返回json**

```json
{
  "code":200,
  "msg":"委托成功",
  "time":1536306331399,
  "data":{
    "ID":18194813
  }
}
```


### 订单取消

注：撤销订单请求为异步报单模式，需要调用/v1/order/detailById接口查询订单状态进行确认。 POST /v1/order/cancel

**请求参数：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
id|y|bigint	|委单id	| |

**返回字段：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||成功：200，失败：300
msg|y|string|消息||
time|y|long|当前毫秒数||

返回json

```json
{
   "code": 200,
   "msg": "取消成功",
   "time": 1536306495984,
   "data": null
}
```



### 委单详情

**HTTP 请求**

- GET /v1/order/detailById


**请求参数：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
id|y|bigint	|委单id	| |
leverAcctid	|n|string	|非杠杆下单无需传词字段，杠杆子账户id，对应开户接口的clientId| |

**返回字段：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||
msg|n|string|消息||
time|y|long|当前毫秒数||
data|y|object|委单详情||

**data：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
types|y|string|委单类型| |买单 、卖单
leftcount|y|number|未成交||
fees|y|number|手续费||
last|y|number|当前委单最新成交价||
count|y|number|数量||
successamount|y|number|已成交总价	||
source|y|string|来源	| |API、WEB、APP
type|y|int|类型代码| |0（买单），1（卖单）
price|y|number|价钱	||
buysymbol|n|string|买符号||
sellsymbol|n|string|卖符号||
time|y|string|创建时间||
statusCode|y|int|状态码| |1 未成交 2 部分成交 3 完全成交 4 撤单处理中 5 已撤销
status|y|int|状态| |未成交、部分成交、完全成交、撤单处理中、已撤销

返回json

```json
{
  "code": 200,
  "msg": "成功",
  "time": 1536306896294,
  "data":    {
    "types": "买单",
    "leftcount": 0.01,
    "fees": 0,
    "last": 0,
    "count": 0.01,
    "successamount": 0,
    "source": "API",
    "type": 0,
    "price": 40000,
    "buysymbol": "",
    "id": 18194814,
    "time": "2018-09-07 15:48:44",
    "sellsymbol": "",
    "statusCode":1,
    "status": "未成交"
  }
}

```


### 成交详情

**HTTP 请求**

- GET /v1/order/counterpartiesById

**请求参数：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
id|y|bigint	|委单id	| |

**返回字段：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||
msg|n|string|消息||
time|y|long|当前毫秒数||
data|y|object|委单详情||

**data：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
entrusts|y|array(object)|对手单列表||

**wallet：**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
id|y|bigint|主键ID||
isSelfTrade|y|int|是否自成交 0 否 1 是||
sysmbol|y|string|交易对||
entrustType|y|int|委单类型 0 买单 1 卖单||
entrustId|y|bigint|委单ID||
matchId|y|bigint|成交ID||
amount|y|number|成交总价||
prize|y|number|价格||
count|y|number|数量||
createTime|y|string|创建时间||


返回json

```json
{
  "code":200,
  "data":{
    "entrusts":[
      {
        "amount":1.2042000000,
        "count":2.2300000000,
        "createTime":"2019-05-27 18:15:12",
        "entrustId":431879850,
        "entrustType":0,
        "id":101192723,
        "isSelfTrade":1,
        "matchId":431879852,
        "prize":0.5400000000,
        "sysmbol":"btc_gavc"
      }
    ]
  },
  "msg":"成功",
  "time":1568690580787
}


```
