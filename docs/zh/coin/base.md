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
