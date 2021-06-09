---
sort: 1 # follow a certain sequence of letters or numbers
---
# 简介1
欢迎使用热币 API！  
此文档是热币API的唯一官方文档，热币API提供的功能和服务会在此文档持续更新，并会发布公告进行通知，建议您关注和订阅我们的公告，及时获取相关信息。

以下是现货API文档各章节主要内容:

### 接入准备1
如需使用API ，请先登录网页端，完成API key的申请和权限配置，再据此文档详情进行开发和交易。

权限说明如下：
* **读取权限**：读取权限用于对数据的查询接口，例如：订单查询、成交查询等。
* **交易权限**：交易权限用于下单、撤单、划转类接口。
* **提币权限**：提币权限用于创建提币订单、取消提币订单操作。

### 安全认证1
AccessKey为API 访问密钥，SecretKey为用户对请求进行签名的密钥。 重要提示：这两个密钥与账号安全紧密相关，无论何时都请勿向其它人透露

<b>示例</b>

 [Java](https://github.com/hotcoinex/openapi/blob/master/ApiDemo.java)  | [Python3](https://github.com/hotcoinex/openapi/blob/master/ApiDemo.py) |  [Php](https://github.com/hotcoinex/openapi/blob/master/Demo.php)


### 合法请求结构
基于安全考虑，除行情API 外的 API 请求都必须进行签名运算。一个合法的请求由以下几部分组成：

- 方法请求地址,即访问服务器地址：api.hotcoinfin.com后面跟上方法名，比如api.hotcoinfin.com/v1/order/place。 

- API 访问密钥（AccessKeyId） 您申请的 APIKEY 中的AccessKey。

- 签名方法（SignatureMethod） 用户计算签名的基于哈希的协议，此处使用 HmacSHA256。
  
- 签名版本（SignatureVersion） 签名协议的版本，此处使用2。

- 时间戳（Timestamp） 您发出请求的时间 (UTC 时区)。在查询请求中包含此值有助于防止第三方截取您的请求。如：2017-05-11T16:22:06.123Z。再次强调是 (UTC 时区)
调用的必需参数和可选参数。可以在每个方法的说明中查看这些参数及其含义。

- 签名计算得出的值，用于确保签名有效和未被篡改。


> https://api.hotcoinfin.com/v1/order/place?  
AccessKeyId=AccessKeyHotcoin123456789  
&symbol=btc_gavc  
&type=buy  
&tradePrice=40000  
&tradeAmount=0.1  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16:22:06.123Z  
&Signature=calculated value  



### 签名运算
API 请求在通过 Internet 发送的过程中极有可能被篡改。为了确保请求未被更改，我们会要求用户在每个请求中带上签名，来校验参数或参数值在传输途中是否发生了更改。

计算签名所需的步骤：
规范要计算签名的请求
因为使用 HMAC 进行签名计算时，使用不同内容计算得到的结果会完全不同。所以在进行签名计算前，请先对请求进行规范化处理。下面以下单请求为例进行说明 


>https://api.hotcoinfin.com/v1/order/place?  
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


>AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16:22:06.123Z  
&symbol=btc_gavc  
&type=buy  
&tradePrice=40000  
&tradeAmount=0.1  

这些参数会被排序为：

>AccessKeyId=AccessKeyHotcoin123456789  
SignatureMethod=HmacSHA256  
SignatureVersion=2  
Timestamp=2017-05-11T16%3A22%3A06.123Z&  
symbol=btc_gavc  
tradeAmount=0.01  
tradePrice=40000  
type=buy  

按照以上顺序，将各参数使用字符’&’连接。 组成最终的要进行签名计算的字符串如下：


>GET\n  
api.hotcoinfin.com\n  
/v1/order/place\n  
AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16%3A22%3A06.123Z  
&symbol=btc_gavc  
&tradeAmount=0.1  
&tradePrice=40000  
&type=buy  


计算签名，将以下两个参数传入加密哈希函数： 要进行签名计算的字符串

>GET\n  
api.hotcoinfin.com\n  
/v1/order/place\n  
AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16%3A22%3A06.123Z  
&symbol=btc_gavc  
&tradeAmount=0.1  
&tradePrice=40000  
&type=buy  

进行签名的密钥（SecretKey）

SecretKeyHotcoin123456789

得到签名计算结果并进行 Base64编码

2oEC+yhkHTsNkgPUq4ZB/5mlY7EZAtUDWOQ5EO01D+I=

将上述值作为参数Signature的取值添加到 API 请求中。 将此参数添加到请求时，必须将该值进行 URI 编码。

symbol 规则： 基础币种+计价币种。如BTC/USDT，symbol为btc_usdt；ETH/BTC， symbol为eth_btc。以此类推。


最终，发送到服务器的 API 请求应该为：

>https://api.hotcoinfin.com/v1/order/place  
?AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16%3A22%3A06.123Z  
&symbol=btc_gavc  
&tradeAmount=0.1  
&tradePrice=40000&type=buy  
&Signature=2oEC%2ByhkHTsNkgPUq4ZB%2F5mlY7EZAtUDWOQ5EO01D%2BI%3D  
