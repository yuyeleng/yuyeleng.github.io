---
sort: 5 # follow a certain sequence of letters or numbers
---
# 账户相关

&nbsp;

### 获取用户余额

- GET /v1/balance

**请求参数:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
AccessKeyId|y|string|访问key
SignatureVersion|y|string|版本
SignatureMethod|y|string|签名方法||HmacSHA256
Signature|y|string|ApiSecret
Timestamp|y|string|时间戳

**响应数据:**


```json
{
   "code": 200,
   "msg": "成功",
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
			"coinName":"比特币",
			"shortName":"BTC"
		},
		{
			"uid":1100011,
			"coinId":2,
			"symbol":"LTC",
			"total":1000.0000000000,
			"frozen":1000.0000000000,
			"coinName":"莱特币",
			"shortName":"LTC"
		},
		{
			"uid":1100011,
			"coinId":4,
			"symbol":"ETH",
			"total":1000.0000000000,
			"frozen":0E-10,
			"coinName":"以太坊",
			"shortName":"ETH"
		}
      ],
      "totalassets": 0
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
netassets|y|number|净资产，单位为gavc
totalassets|y|number|总资产，单位为gavc
wallet|y|array(object)|钱包列表

**wallet:**

参数名称|是否必须|类型|描述|默认值|取值范围
------------- | ------------- |  ------------- | ------------- |  ------------- | -------------
coinName|y|long|币种名称
uid|y|int|用户ID
coinId|y|int|币种ID
total|y|number|可用
frozen|y|number|冻结
symbol|y|string|币种symbol
shortName|y|string|币种简称


