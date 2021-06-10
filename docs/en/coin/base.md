---
sort: 3 # follow a certain sequence of letters or numbers
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

