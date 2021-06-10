---
sort: 5 # follow a certain sequence of letters or numbers
---
# Account

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

