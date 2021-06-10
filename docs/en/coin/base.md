---
sort: 2 # follow a certain sequence of letters or numbers
---
# Reference Data

### Get all trade pairs


```json
https://hkapi.hotcoin.top/v1/common/symbols

curl "https://hkapi.hotcoin.top/v1/common/symbols"
```

- GET /v1/common/symbols  

**Request parameters:**   

**Response data:**  

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code||success：200
msg|y|string|message||
time|y|long|Current millisseconds||
data|y|array|symbols list||


**data:**

```json
{
    "code":"200",
    "time":1567045034,
    "data":[
        {
            "baseCurrency":"etc",
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

Field Type|Data Type|description
------------- | -------------  | ------------
baseCurrency|string|baseCurrency code
quoteCurrency|string|quoteCurrency code
pricePrecision|integer|price precision
amountPrecision|integer|quantity precision
symbolPartition|string|symbol Partition, example:[main,innovation]
symbol|string|trade pair code
state|string|trade pair status [enable,disable]
minOrderCount|decimal|min order count
maxOrderCount|decimal|max order count
minOrderPrice|decimal|min order price
maxOrderPrice|decimal|max order price

&nbsp;

### Place order

- POST /v1/order/place

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access Key	|||
SignatureVersion|y|string|Version|| |
SignatureMethod|y|string|Signature Method| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|Timestamp||
symbol|y|string|Trading pair| |example：btc_usdt
type|y|string|Type| | "buy" ,”sell"
tradeAmount|y|number|amount||
tradePrice|y|number	|price||



**Response data:**

```json
{
  "code":200,
  "msg":"order success",
  "time":1536306331399,
  "data":{
    "ID":18194813
  }
}
```

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code||success：200，failed：300
msg|y|string|message||
time|y|long|Current millisseconds||
data|y|object|data||


**msg range**

English |
 | ------------ 
|Illegal request 
| Illegal tradeAmount value 
| Illegal tradePrice value 
| Illegal symbol format

**data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
ID|y|bigint|order id||

&nbsp;

### Order Cancel

Note:Cancel order requests is under asynchronous pattern,call interface /v1/order/detailById is required for order status query.  


- POST /v1/order/cancel

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key	|||
SignatureVersion|y|string|Version|| |
SignatureMethod|y|string|Signature Method| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|Timestamp||
id|y|bigint	|Order id	| |

**Response data:**

```json
{
   "code": 200,
   "msg": "Cancel Success",
   "time": 1536306495984,
   "data": null
}
```

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code||success：200，failed：300
msg|y|string|message||
time|y|long|Current millisseconds||


&nbsp;

### Order Details



- GET /v1/order/detailById


**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key	|||
SignatureVersion|y|string|Version|| |
SignatureMethod|y|string|Signature Method| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|Timestamp||
id|y|bigint	|order id	| |
leverAcctid	|n|string	|Fields not required innon-leverorder,Sub-account id,clientId in line with API| |

**Response data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code||
msg|n|string|message||
time|y|long|Current millisseconds||
data|y|object|order details||

**data:**

```json
{
  "code": 200,
  "msg": "success",
  "time": 1536306896294,
  "data":    {
    "types": "bug",
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
    "status": "unsettled"
  }
}

```


Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
types|y|string|order type| |Buy 、Sell
leftcount|y|number|Unfill||
fees|y|number|Fee||
last|y|number|Current order latest price||
count|y|number|amount||
successamount|y|number|Total transaction||
source|y|string|Source	| |API、WEB、APP
type|y|int|Data Type code| |0（Buy），1（Sell）
price|y|number|Price	||
buysymbol|n|string|Buy symbol||
sellsymbol|n|string|Sell symbol||
time|y|string|Establish time||
statusCode|y|int|Status code| |1 Unfilled 2 Partial filled 3 Filled 4 Revoking 5 Cancelled
status|y|int|Status| |Unfill,Partial filled,Filled,Revoking,Cancelled


&nbsp;


### Transaction details


- GET /v1/order/counterpartiesById

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key	|||
SignatureVersion|y|string|Version|| |
SignatureMethod|y|string|Signature Method| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|Timestamp||
id|y|bigint	|order id	| |

**Response data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code||
msg|n|string|message||
time|y|long|Current millisseconds||
data|y|object|Order detail||

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
  "msg":"success",
  "time":1568690580787
}
```


Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
entrusts|y|array(object)|BBO list||

**wallet:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
id|y|bigint|Primary key ID||
isSelfTrade|y|int|Self-trade Option 0 n 1 y||
sysmbol|y|string|Trading pairs||
entrustType|y|int|orderData Type 0 Buy 1 Sell||
entrustId|y|bigint|order id||
matchId|y|bigint|Transaction ID||
amount|y|number|Total Transaction||
prize|y|number|Price||
count|y|number|Amount||
createTime|y|string|Create time||



&nbsp;

### Obtain order list


- GET /v1/order/entrust

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key	|||
SignatureVersion|y|string|Version|| |
SignatureMethod|y|string|Signature Method| |HmacSHA256
Signature|y|string|ApiSecret||
Timestamp|y|string|Timestamp||
symbol|y|string	|Trading pairs| |example：btc_usdt
type|n|int|Data Type|0|0 is all 1 iscurrent 2 is history
page|n|int|page|1|
count|y|int|items|7|[1-100] max 100 items


**Response data:**

```json
{
  "code": 200,
  "msg": "Obtain success！",
  "time": 1527841588334,
  "data":{
    "entrutsHis": [
      {
        "types": "Buy",
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
        "status": "Cancelled"
      },
      {
        "types": "Buy",
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
        "status": "Cancelled"
      }
    ],
    "entrutsCur": [
      {
        "types": "Buy",
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
        "status": "Unfill"
      },
      {
        "types": "Sell",
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
        "status": "Unfill"
      }
    ]
  }
}

```

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code||
msg|n|string|Return message||
time|y|long|Current millisseconds||
data|y|object|Order details||

**data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
entrutsCur|n|array(object)|Current order||
entrutsHis|n|array(object)|History order||

**entrutsCur 及 entrutsHis Data Type is the same:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
id|y|bigint|Order id
time|y|string|Order time
types|y|string|Order Type|| Buy、Sell
source|y|string|Order source||"WEB"，"APP"，"API"
price|y|number|Order price
count|y|number|Order amount
leftcount|y|number|Unfill amount
last|y|number|Transaction price
successamount|y|number|Total Transaction
fees|y|number|fee
status|y|string|Order Status||Unfill,Partial filled,Filled,Revoking,Cancelled
type|y|int|Order Data Type||	0( "Buy"),1( "Sell")
buysymbol|y|string|Currency Data Type symbol
sellsymbol|y|string|Currency Data Type symbol

&nbsp;

### Current&History Transaction Record

- GET /v1/order/matchresults


**Request parameters:**


Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key
SignatureVersion|y|string|Version
SignatureMethod|y|string|Signature Method||HmacSHA256
Signature|y|string|ApiSecret
Timestamp|y|string|Timestamp
symbol|y|string|Trading pairs||example：btc_usdt
types|n|string|query order  Data Type compositio，separate by','||0：buy, 1：sell
startDate|n|string|Inquire start date, date form yyyy-mm-dd|-1d Inquire the day before end data|Value Range [((endDate) – 1), (endDate)] ，Max inquery date is 2 days in query window, inquery ranges of nearly 61 days
endDate|n|string|Inqery end date, date form yyyy-mm-dd|today|Value Range [(today-60), today] ，Max inquery date is 2 days in query window, inquery ranges of nearly 61 days
from|n|string|Inquire start&end  ID|order history record ID（max）|
direct|n|string|Direction|default next， transaction record ID orders from big to small|prev forward，time（or ID）positive sequence；next backward，time（or ID）reverse）
size|n|string| query record scope|100|[1，100]

**Response data:**


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
  "msg":"success",
  "time":1568690580787
}
```


Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|return message
time|y|long|Current millisseconds
data|y|object|Real-time transactions

**data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
entrustdetail|n|array(object)|Transaction Record


**entrustdetail:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
createdAt|y|long|Transaction time
filledAmount|y|string|Transaction amount
filledFees|y|string|Transaction fee
id|y|long|Transaction record id
matchId|y|long|Matchmaking id
orderId|y|long|Order id
price|y|string|Transaction price
type|y|string|Order Data Type||0：buy, 1：sell
role|y|string|Transaction role||taker,maker


&nbsp;

### Batch Cancels

- POST /v1/order/batchCancelOrders

`Note： The API only for submit cancel request,actual result need to confirm by order Status,matchmaking Status.`

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
orderIds|y|String|Revoke order ID list||Shall not exceeds 100 order ID each time Example"2232,1232,2321"

**Response data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|Return message
data|y|object|

&nbsp;

### Batch Cancels(OpenOrders)


- POST /v1/order/batchCancelOpenOrders

`Note： The API only for submit cancel request,actual result need to confirm by order Status,matchmaking Status.`

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
symbol|y|String|Transaction code list( Max 10 symbols,separate by commas between various trade codes)，btc_usdt, eth_btc...||
side|n|String|Direction||when buy -buy direction sell -sell direction is empty，obtain orders of all directions to revoke.

**Response data:**

```json
{
  "code":200,
  "data":{
    "successCount": 1,
    "failCount": 1
  },
  "msg":"success"
}
```

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|Return message
data|y|object|

**data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
{successCount|y|int|Success cancel amount
failCount}|y|int|Cancelled failed amount

&nbsp;

### Batch Orders

API Key Access：Trading,Maximum 10 orders for a single batch


- POST /v1/order/batchOrders

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
orders|y|object|Order list||

**orders:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
[{symbol|y|string|Trading pairs||example：btc_usdt
type|y|string|Data Type||"buy" ,”sell"
tradeAmount|y|number|Amount||
tradePrice}]|y|number|Price||

**Response data:**

```json
{
  "code": 200,
  "msg": "success",
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


Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|Return message
data|y|object|

**data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
[{ID|y|bigint|Order id||
errcode|n|string|return error code
errmsg}]|n|string|Return error Description


&nbsp;

### Obtain User Balance

- GET /v1/balance

**Request parameters:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|Access key
SignatureVersion|y|string|Version
SignatureMethod|y|string|Signature Method||HmacSHA256
Signature|y|string|ApiSecret
Timestamp|y|string|Timestamp

**Response data:**


```json
{
   "code": 200,
   "msg": "success",
   "time": 1527835756743,
   "data":    {
      "netassets": 0,
      "wallet": [
         {
			"uid":1100011,
			"coinId":1,
			"symbol":"BTC",
			"total":1000.0000000000,
			"frozen":1000.0000000000,
			"coinName":"BTC",
			"shortName":"BTC"
		},
		{
			"uid":1100011,
			"coinId":2,
			"symbol":"LTC",
			"total":1000.0000000000,
			"frozen":1000.0000000000,
			"coinName":"LTC",
			"shortName":"LTC"
		},
		{
			"uid":1100011,
			"coinId":4,
			"symbol":"ETH",
			"total":1000.0000000000,
			"frozen":0E-10,
			"coinName":"ETH",
			"shortName":"ETH"
		}
      ],
      "totalassets": 0
   }
}

```

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
code|y|int|Status code
msg|n|string|Return message
time|y|long|Current millisseconds
data|y|object|Transaction deep data


**data:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
netassets|y|number|net assets，unit: gavc
totalassets|y|number|total assets，unit:gavc
wallet|y|array(object)|wallet list

**wallet:**

Parameter|Mandatory|Data Type|Description|Default|Value Range
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
coinName|y|long|Currency
uid|y|int|User ID
coinId|y|int|Currency ID
total|y|number|Available
frozen|y|number|On orders
symbol|y|string|Currency symbol
shortName|y|string|Abbreviation

