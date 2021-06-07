---
# 简介
欢迎使用热币 API！
此文档是热币API的唯一官方文档，热币API提供的功能和服务会在此文档持续更新，并会发布公告进行通知，建议您关注和订阅我们的公告，及时获取相关信息。

以下是现货API文档各章节主要内容

### 快速入门
### 接入准备
如需使用API ，请先登录网页端，完成API key的申请和权限配置，再据此文档详情进行开发和交易。

权限说明如下：
读取权限：读取权限用于对数据的查询接口，例如：订单查询、成交查询等。
交易权限：交易权限用于下单、撤单、划转类接口。
提币权限：提币权限用于创建提币订单、取消提币订单操作。

创建成功后请务必记住以下信息：
Access Key API 访问密钥
Secret Key 签名认证加密所使用的密钥（仅申请时可见）

<b>SDK示例（推荐）</b>

 [Java](https://github.com/hotcoinex/openapi/blob/master/ApiDemo.java)  | [Python3](https://github.com/hotcoinex/openapi/blob/master/ApiDemo.py) |  [Php](https://github.com/hotcoinex/openapi/blob/master/Demo.php)

### 安全认证
AccessKey为API 访问密钥，SecretKey为用户对请求进行签名的密钥。 重要提示：这两个密钥与账号安全紧密相关，无论何时都请勿向其它人透露
### 合法请求结构
基于安全考虑，除行情API 外的 API 请求都必须进行签名运算。一个合法的请求由以下几部分组成：
方法请求地址,即访问服务器地址：api.hotcoinfin.com后面跟上方法名，比如api.hotcoinfin.com/v1/order/place。
API 访问密钥（AccessKeyId） 您申请的 APIKEY 中的AccessKey。

签名方法（SignatureMethod） 用户计算签名的基于哈希的协议，此处使用 HmacSHA256。
签名版本（SignatureVersion） 签名协议的版本，此处使用2。

时间戳（Timestamp） 您发出请求的时间 (UTC 时区)。在查询请求中包含此值有助于防止第三方截取您的请求。如：2017-05-11T16:22:06.123Z。再次强调是 (UTC 时区)
调用的必需参数和可选参数。可以在每个方法的说明中查看这些参数及其含义。签名计算得出的值，用于确保签名有效和未被篡改。
例：
```java
https://api.hotcoinfin.com/v1/order/place?
AccessKeyId=AccessKeyHotcoin123456789
&symbol=btc_gavc
&type=buy
&tradePrice=40000
&tradeAmount=0.1
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T16:22:06.123Z
&Signature=calculated value
```


### 签名运算
API 请求在通过 Internet 发送的过程中极有可能被篡改。为了确保请求未被更改，我们会要求用户在每个请求中带上签名，来校验参数或参数值在传输途中是否发生了更改。

计算签名所需的步骤：
规范要计算签名的请求
因为使用 HMAC 进行签名计算时，使用不同内容计算得到的结果会完全不同。所以在进行签名计算前，请先对请求进行规范化处理。下面以下单请求为例进行说明

https://api.hotcoinfin.com/v1/order/place?
AccessKeyId=AccessKeyHotcoin123456789
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T16:22:06.123Z
&symbol=btc_gavc
&type=buy
&tradePrice=40000
&tradeAmount=0.1

请求方法（GET 或 POST），后面添加换行符\n。

GET\n

添加小写的访问地址，后面添加换行符\n。

api.hotcoinfin.com\n

访问方法的路径，后面添加换行符\n。

/v1/order/place\n

按照ASCII码的顺序对参数名进行排序(使用 UTF-8 编码，且进行了 URI 编码，十六进制字符必须大写，如‘:’会被编码为'%3A'，空格被编码为'%20')。
例如，下面是请求参数的原始顺序，进行过编码后。

AccessKeyId=AccessKeyHotcoin123456789
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T16:22:06.123Z
&symbol=btc_gavc
&type=buy
&tradePrice=40000
&tradeAmount=0.1

这些参数会被排序为：

AccessKeyId=AccessKeyHotcoin123456789
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T16%3A22%3A06.123Z&
symbol=btc_gavc
tradeAmount=0.01
tradePrice=40000
type=buy

按照以上顺序，将各参数使用字符’&’连接。

AccessKeyId=AccessKeyHotcoin123456789&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T16%3A22%3A06.123Z&symbol=btc_gavc&tradeAmount=0.1&tradePrice=40000&type=buy

组成最终的要进行签名计算的字符串如下：

GET\n
api.hotcoinfin.com\n
/v1/order/place\n
AccessKeyId=AccessKeyHotcoin123456789&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T16%3A22%3A06.123Z&symbol=btc_gavc&tradeAmount=0.1&tradePrice=40000&type=buy

计算签名，将以下两个参数传入加密哈希函数：
要进行签名计算的字符串

GET\n
api.hotcoinfin.com\n
/v1/order/place\n
AccessKeyId=AccessKeyHotcoin123456789&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T16%3A22%3A06.123Z&symbol=btc_gavc&tradeAmount=0.1&tradePrice=40000&type=buy

进行签名的密钥（SecretKey）

SecretKeyHotcoin123456789

得到签名计算结果并进行 Base64编码

2oEC+yhkHTsNkgPUq4ZB/5mlY7EZAtUDWOQ5EO01D+I=

将上述值作为参数Signature的取值添加到 API 请求中。 将此参数添加到请求时，必须将该值进行 URI 编码。
最终，发送到服务器的 API 请求应该为：

https://api.hotcoinfin.com/v1/order/place?AccessKeyId=AccessKeyHotcoin123456789&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T16%3A22%3A06.123Z&symbol=btc_gavc&tradeAmount=0.1&tradePrice=40000&type=buy&Signature=2oEC%2ByhkHTsNkgPUq4ZB%2F5mlY7EZAtUDWOQ5EO01D%2BI%3D

symbol 规则： 基础币种+计价币种。如BTC/USDT，symbol为btc_usdt；ETH/BTC， symbol为eth_btc。以此类推。

### 基础信息
### 简介 
基础信息Rest接口提供了获取所有交易对信息。

### 获取所有交易对
此接口返回所有热币网支持的交易对。

https://hkapi.hotcoin.top/v1/common/symbols

curl "https://hkapi.hotcoin.top/v1/common/symbols"

HTTP 请求

GET /v1/common/symbols

请求参数

此接口不接受任何参数

返回字段：

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码||成功：200
msg|y|string|消息||
time|y|long|当前毫秒数||
data|y|array|symbols列表||


data结构

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
### 行情接口
### 简介
行情接口提供了多种K线、深度以及最新成交记录等行情数据。

- [实时ticker数据 /v1/market/ticker](#实时ticker数据)
- [获取k线数据 /v1/ticker](#获取k线数据)
- [获取深度数据 /v1/depth](#获取深度数据)
- [获取实时成交数据 /v1/trade](#获取实时成交数据)

### API明细

### 实时ticker数据
HTTP 请求
* GET /v1/market/ticker



请求参数：

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
status|y|string|状态码||成功：ok，失败：error
timestamp|y|long|当前毫秒数||
ticker|y|list|数据||

响应数据:

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


返回json

```json
{
   "status": "ok",
   "timestamp": 1567045034,
   "ticker":[
			{
				"symbol":"btc_usdt",
				"last":10000.00000000,
				"buy":9999.00000000,
				"sell":10001.00000000,
				"high":11000.00000000,
				"low":9000.00000000,
				"vol":10000000.0000,
				"change":10.10 
			}
		]
}
```
### 获取k线数据
HTTP 请求
* GET /v1/ticker

请求参数：

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
step|y|int|时间：秒||60（1分钟）,300（5分钟）,900（15分钟）,1800（30分钟）,3600（1小时）,86400（1天）,604800（1周）,2592000（1月）
symbol|y|string|交易对||例：btc_gavc

响应数据 :

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
time|y|long|当前毫秒数
data|y|array(array(number))|K线数据

data<br>

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


返回json

```json
{
   "code": 200,
   "msg": "成功",
   "time": 1527838104874,
   "data": [
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

### 获取深度数据
HTTP 请求

* GET /v1/depth

请求参数：

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
symbol|y|string|交易对||例：btc_gavc
step|n|int|加上此参数可查最新一个k线数据，类型为时间，单位秒||60,3*60,5*60,15*60,30*60,60*60（1小时）,24*60*60（1天）,7*24*60*60（1周）,30*24*60*60（1月）


响应数据:

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
time|y|long|当前毫秒数
data|y|object|交易深度数据

data

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
depth|y|object
period|n|object|传step时才有值

depth

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
bids|y|array(array(long))|买盘,[price(成交价), amount(成交量)]
asks|y|array(array(long))|卖盘,[price(成交价), amount(成交量)]
date|y|long|时间戳
lastPrice|y|number|最新成交价

period

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
marketFrom|y|string|入参symbol
coinVol|y|string|入参symbol
type|y|long|入参step,时间
data|y|array（array）|最后一个k线数据，格式同上，但只有一个



返回json

```json
{
   "code": 200,
   "msg": "成功",
   "time": 1527837164605,
   "data":{
      "period":{
         "data": [[
            1527837120000,
            54598.5,
            54598.5,
            54598.5,
            54598.5,
            0
         ]],
         "marketFrom": "btc_gavc",
         "type": 60,
         "coinVol": "btc_gavc"
      },
      "depth":{
         "date": 1527837163,
         "asks": [
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
	 "bids": [
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
         "lastPrice": 54598.5
      }
   }
}
```
### 获取实时成交数据
HTTP 请求

* GET /v1/trade

参数：

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
count|y|int|Trades条数||0
symbol|y|string|交易对||例：btc_gavc

返回 :

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|状态码
msg|n|string|返回消息
time|y|long|当前毫秒数
data|y|object|实时成交数据

data

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
trades|y|array(object)|trades数据
sellSymbol|y|string|sellSymbol
buySymbol|y|string|buySymbol

trades

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
price|y|long|成交价钱
amount|y|string|成交数量
id|y|string|成交id
time|y|string|成交时间
en_type|y|string|成交方向||"bid"(买入),"ask"(卖出)
type|y|string|成交类型||"买入","卖出"

返回json

```json
{
 "code": 200,
 "msg": "成功",
   "time": 1536315868962,
   "data":    {
     "sellSymbol": "BTC",
     "buySymbol": "GAVC",
     "trades": [
       {
          "price": 0.007,
          "amount": 66491.04,
          "id": 1,
          "time": "02:45:08",
          "en_type": "ask",
          "type": "卖出"
       }, 
	   {
          "price": 0.007,
          "amount": 66491.04,
          "id": 1,
          "time": "02:45:08",
          "en_type": "ask",
          "type": "卖出"
       }

	]
   }
}
```

### 链接
QQ浏览器 | [点击打开](https://browser.qq.com/?adtag=SEM170314020/)    |   搜索资料常用
有道词典 | [link](http://cidian.youdao.com/)    |   开发中翻译