---
sort: 1 # follow a certain sequence of letters or numbers
---
# Introduction

Welcome to Hotcoin API！

This is the official Hotcoin API document, and will be continue updating. Huobi will also publish API announcement in advance for any API change. Please subscribe to our announcements so that you can get the latest updates.

Below is the content for Spot API document:

### Access Instructions

Before you use API, you need to login the website to create API Key with proper permissions. The API key is shared for all instruments in Huobi including spot, futures, swap, options.  

The permissions are described as follows:
* **Read permission**:It is used to query the data, such as order query, trade query.
* **Trade permission**:It is used to create order, cancel order and transfer, etc.
* **Withdraw permission**:It is used to create withdraw order, cancel withdraw order, etc.

### Security Authentication

AccessKey is the access key to API,SecretKey is the secret key of user signature for requests. Important note: These two keys are closely related to account security, and should not be disclosed to others at any time  

<b>Demo</b>

 [Java](https://github.com/hotcoinex/openapi/blob/master/ApiDemo.java)  | [Python3](https://github.com/hotcoinex/openapi/blob/master/ApiDemo.py) |  [Php](https://github.com/hotcoinex/openapi/blob/master/Demo.php)


### Legal request structure

Based on the consideration of security, all API request must be calculated by signature algorithm except market API. A legel request consists of below parts:  


- Request-URI Method, i.e. the address access to server: hkapi.hotcoin.top followed by method name, take hkapi.hotcoin.top/v1/order/place for instance. 

- API AccessKeyId is the The AccessKey in the APIKEY you requested.

- Signature Method is the hash-based protocol of calculating signatures by users, HmacSHA256 is applied here.
  
- SignatureVersion is the version of the signature protocol, 2 is applied here.

- Timestamp is the time you made the request (UTC Time Zone). Including the value in query request helps preventing third parties from interception of your request.For example：2017-05-11T16:22:06.123Z。Again,(UTC Time Zone) is stressed here.For required parameters and optional parameters ,these parameters and their meanings can be viewed in the description of each method.

- Signature the value calculated by signature is used for ensuring that the signature is valid and not tampered.  


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



### Signature Algorithm
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
