---
sort: 6 # follow a certain sequence of letters or numbers
---
# 现货/杠杆交易

### 下单

- POST /v1/order/place

**请求参数:**

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



**响应数据:**

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

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||成功：200，失败：300
msg|y|string|消息||
time|y|long|当前毫秒数||
data|y|object|数据||


**msg 范围**

中文 | English |
------------ | ------------ 
非法请求 |Illegal request 
请使用正确的数量| Illegal tradeAmount value 
请使用正确的价格| Illegal tradePrice value 
币种ID错误| Illegal symbol format

**data:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
ID|y|bigint|订单id||

&nbsp;

### 订单取消

注：撤销订单请求为异步报单模式，需要调用/v1/order/detailById接口查询订单状态进行确认。

- POST /v1/order/cancel

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
id|y|bigint	|委单id	| |

**响应数据:**

```json
{
   "code": 200,
   "msg": "取消成功",
   "time": 1536306495984,
   "data": null
}
```

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||成功：200，失败：300
msg|y|string|消息||
time|y|long|当前毫秒数||


&nbsp;

### 委单详情



- GET /v1/order/detailById


**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
id|y|bigint	|委单id	| |
leverAcctid	|n|string	|非杠杆下单无需传词字段，杠杆子账户id，对应开户接口的clientId| |

**响应数据:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||
msg|n|string|消息||
time|y|long|当前毫秒数||
data|y|object|委单详情||

**data:**

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


&nbsp;


### 成交详情


- GET /v1/order/counterpartiesById

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
id|y|bigint	|委单id	| |

**响应数据:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||
msg|n|string|消息||
time|y|long|当前毫秒数||
data|y|object|委单详情||

**data:**

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


参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
entrusts|y|array(object)|对手单列表||

**wallet:**

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



&nbsp;

### 获取委单列表


- GET /v1/order/entrust

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key	|||
SignatureVersion|y|string|版本|| |
SignatureMethod|y|string|签名方法| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|时间戳||
symbol|y|string	|交易对| |例：btc_usdt
type|n|int|类型|0|0表示全部 1表示当前 2表示历史
page|n|int|页码|1|
count|y|int|条数|7|[1-100] 最大100条


**响应数据:**

```json
{
  "code": 200,
  "msg": "获取成功！",
  "time": 1527841588334,
  "data":{
    "entrutsHis": [
      {
        "types": "买单",
        "leftcount": 1.0E-4,
        "fees": 0,
        "last": 0,
        "count": 1.0E-4,
        "successamount": 0,
        "source": "WEB",
        "type": 1,
        "price": 1.0E7,
        "buysymbol": "GAVC",
        "id": 947644,
        "time": "2018-06-27 17:45:14",
        "sellsymbol": "BTC",
        "status": "已撤销"
      },
      {
        "types": "买单",
        "leftcount": 1.0E-4,
        "fees": 0,
        "last": 0,
        "count": 1.0E-4,
        "successamount": 0,
        "source": "WEB",
        "type": 1,
        "price": 1.0E7,
        "buysymbol": "GAVC",
        "id": 947645,
        "time": "2018-06-27 17:45:14",
        "sellsymbol": "BTC",
        "status": "已撤销"
      }
    ],
    "entrutsCur": [
      {
        "types": "买单",
        "leftcount": 0.01,
        "fees": 0,
        "last": 0,
        "count": 0.01,
        "successamount": 0,
        "source": "API",
        "type": 0,
        "price": 40000,
        "buysymbol": "GAVC",
        "id": 18194814,
        "time": "2018-09-07 15:48:44",
        "sellsymbol": "BTC",
        "status": "未成交"
      },
      {
        "types": "卖单",
        "leftcount": 0.01,
        "fees": 0,
        "last": 0,
        "count": 0.01,
        "successamount": 0,
        "source": "API",
        "type": 0,
        "price": 40000,
        "buysymbol": "GAVC",
        "id": 18194814,
        "time": "2018-09-07 15:48:44",
        "sellsymbol": "BTC",
        "status": "未成交"
      }
    ]
  }
}

```

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||
msg|n|string|消息||
time|y|long|当前毫秒数||
data|y|object|委单详情||

**data:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
entrutsCur|n|array(object)|当前委单||
entrutsHis|n|array(object)|历史委单||

**entrutsCur 及 entrutsHis类型相同:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
id|y|bigint|委单id
time|y|string|下单时间
types|y|string|委单类型|| 买单、卖单
source|y|string|委单来源||"WEB"，"APP"，"API"
price|y|number|下单价格
count|y|number|下单数量
leftcount|y|number|未成交数量
last|y|number|成交价格
successamount|y|number|成交总价
fees|y|number|手续费
status|y|string|委单状态||未成交、部分成交、完全成交、撤单处理中、已撤销
type|y|int|委单类型||	0( "买单"),1( "卖单")
buysymbol|y|string|币种类型符号
sellsymbol|y|string|币种类型符号

&nbsp;

### 当前和历史成交记录

- GET /v1/order/matchresults


**请求参数:**


参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key
SignatureVersion|y|string|版本
SignatureMethod|y|string|签名方法||HmacSHA256
Signature|y|string|ApiSecret
Timestamp|y|string|时间戳
symbol|y|string|交易对||例：btc_usdt
types|n|string|查询的订单类型组合，使用','分割||0：买, 1：卖
startDate|n|string|查询开始日期, 日期格式yyyy-mm-dd|-1d 查询结束日期的前1天|取值范围 [((endDate) – 1), (endDate)] ，查询窗口最大为2天，窗口平移范围为最近61天
endDate|n|string|查询结束日期, 日期格式yyyy-mm-dd|today|取值范围 [(today-60), today] ，查询窗口最大为2天，窗口平移范围为最近61天
from|n|string|查询起始 ID|订单成交记录ID（最大值）|
direct|n|string|查询方向|默认 next， 成交记录 ID 由大到小排序|prev 向前，时间（或 ID）正序；next 向后，时间（或 ID）倒序）
size|n|string| 查询记录大小|100|[1，100]

**响应数据:**


```json
{
  "code":200,
  "data":{
    "entrustdetail":[
      {
        "createdAt":1623134000577,
        "filledAmount":"1.20",
        "filledFees":"2.2300",
        "id":43187,
        "matchId":123456,
        "orderId":431879852,
        "type":"1",
        "price":"0.5400000000",
        "role":"taker"
      }
    ]
  },
  "msg":"成功",
  "time":1568690580787
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
entrustdetail|n|array(object)|成交记录


**entrustdetail:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
createdAt|y|long|成交时间
filledAmount|y|string|成交数量
filledFees|y|string|成交手续费
id|y|long|订单成交记录id
matchId|y|long|撮合id
orderId|y|long|订单id
price|y|string|成交价格
type|y|string|订单类型||0：买, 1：卖
role|y|string|成交角色||taker,maker


&nbsp;

### 批量撤单

- POST /v1/order/batchCancelOrders

`注意：此接口只提交取消请求，实际取消结果需要通过订单状态，撮合状态等接口来确认。`

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
orderIds|y|String|撤销订单ID列表||单次不超过100个订单id 例如 "2232,1232,2321"

**响应数据:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
data|y|object|

&nbsp;

### 批量撤单(OpenOrders)


- POST /v1/order/batchCancelOpenOrders

`注意：此接口只提交取消请求，实际取消结果需要通过订单状态，撮合状态等接口来确认。`

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
symbol|y|String|交易代码列表（最多10 个symbols，多个交易代码间以逗号分隔），btc_usdt, eth_btc...（||
side|n|String|交易方向||buy -买方向 sell -卖方向  为空时，则获取所有方向的委单进行撤销。

**响应数据:**

```json
{
  "code":200,
  "data":{
    "successCount": 1,
    "failCount": 1
  },
  "msg":"成功"
}
```

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
data|y|object|

**data:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
{successCount|y|int|成功撤销数量
failCount}|y|int|撤销失败数量

&nbsp;

### 批量下单

API Key 权限：交易 ,一个批量最多10张订单



- POST /v1/order/batchOrders

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
orders|y|object|订单列表||

**orders:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
[{symbol|y|string|交易对||例：btc_usdt
type|y|string|类型||"buy" ,”sell"
tradeAmount|y|number|数量||
tradePrice}]|y|number|价钱||

**响应数据:**

```json
{
  "code": 200,
  "msg": "成功",
  "time": 1527841588334,
  "data":{
    "list": [
      {
        "ID":123456,
        "errcode": "",
        "errmsg": ""
      },
      {
        "ID":1234567,
        "errcode": "",
        "errmsg": ""
      }
    ]
  }
}
```


参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
data|y|object|

**data:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
[{ID|y|bigint|订单id||
errcode|n|string|返回错误码
errmsg}]|n|string|返回错误描述


