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

- Timestamp is the time you made the request (UTC Time Zone). Including the value in query request helps preventing third parties from interception of your request.For example：2017-05-11T16:22:06.123Z.Again,(UTC Time Zone) is stressed here.For required parameters and optional parameters ,these parameters and their meanings can be viewed in the description of each method.

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
API request can be tempered at great risk while it is sent through the Internet. To ensure that the request is not tempered, we will require users to sign in every request (except market API) for verifying the authticity of parameters or the values of parameters.  


Sign Steps:  

Standards for Requests of Calculating Signatures:  

Establish a standard for requests of calculating signatures because when using HMAC to calculate signature, total different results will be achieved as different contents are calculated. So before calculating signatures, please make a standard. Examples of the request of order placement will be given as follows.

>https://api.hotcoinfin.com/v1/order/place?  
AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16:22:06.123Z  
&symbol=btc_gavc  
&type=buy  
&tradePrice=40000  
&tradeAmount=0.1  

The request Method (GET or POST, WebSocket use GET), append line break "\n"  

GET\n

The host with lower case, append line break \n.

api.hotcoinfin.com\n.

The path, append line break "\n"

/v1/order/place\n

Sorting parameters by order of ASCII code (Encoding by UTF-8 format and URI format，hexadecimal characters must be capitalized，for example‘:’ will be encoded as '%3A'，space key will be encoded as '%20').  

For example，here is the original order of the request parameters after encoded.


>AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16:22:06.123Z  
&symbol=btc_gavc  
&type=buy  
&tradePrice=40000  
&tradeAmount=0.1  

Above parameter should be ordered like below:

>AccessKeyId=AccessKeyHotcoin123456789  
SignatureMethod=HmacSHA256  
SignatureVersion=2  
Timestamp=2017-05-11T16%3A22%3A06.123Z&  
symbol=btc_gavc  
tradeAmount=0.01  
tradePrice=40000  
type=buy  

Use char “&” to concatenate all parameters.

>AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16%3A22%3A06.123Z  
&symbol=btc_gavc  
&tradeAmount=0.1  
&tradePrice=40000  
&type=buy

The final strings for signature calculation is as follows:


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


Calculation of signature algorithm,transfer the following two parameters to the cryptographic hash function:  

Strings for signature calculation  

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

SecretKey for execute signature

SecretKeyHotcoin123456789

Obtained result of signature calculation and encoded with Base64

2oEC+yhkHTsNkgPUq4ZB/5mlY7EZAtUDWOQ5EO01D+I=

Add the above values as the value of parameter Signature to the API request. The value must be encoded with URI when this parameter is added to the request.  


symbol rules：base currency + quote currency.In BTC/USDT，symbol is btc_usdt；While in ETH/BTC， symbol is eth_btc, and so on.  

Finally, the API request sent to the server should be：

>https://api.hotcoinfin.com/v1/order/place  
?AccessKeyId=AccessKeyHotcoin123456789  
&SignatureMethod=HmacSHA256  
&SignatureVersion=2  
&Timestamp=2017-05-11T16%3A22%3A06.123Z  
&symbol=btc_gavc  
&tradeAmount=0.1  
&tradePrice=40000&type=buy  
&Signature=2oEC%2ByhkHTsNkgPUq4ZB%2F5mlY7EZAtUDWOQ5EO01D%2BI%3D  
