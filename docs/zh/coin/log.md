---
sort: 1 # follow a certain sequence of letters or numbers
---
# 更新日志

&nbsp;

生效时间(UTC +8)|接口|变化|摘要|
------------- | ------------- |  ------------- | ------------- |
2021.6.10|GET /v1/common/symbols |新增|获取所有交易对
2021.6.10|POST /v1/order/place|新增|新增下单接口
2021.6.10|POST /v1/order/cancel|新增|新增订单取消接口，注：撤销订单请求为异步报单模式，需要调用/v1/order/detailById接口查询订单状态进行确认。
2021.6.10|GET /v1/order/detailById|新增|新增委单详情接口
2021.6.10|GET /v1/order/counterpartiesById|新增|新增成交详情接口
2021.6.10|GET /v1/order/entrust|新增|新增获取委单列表接口
2021.6.10|GET /v1/order/matchresults|新增|新增当前和历史成交记录接口
2021.6.10|POST /v1/order/batchCancelOrders|新增|新增批量撤单接口
2021.6.10|POST /v1/order/batchCancelOpenOrders|新增|新增批量撤单(OpenOrders)接口
2021.6.10|POST /v1/order/batchOrders|新增|新增批量下单接口，API Key 权限：交易 ,一个批量最多10张订单
2021.6.10|GET /v1/balance|新增|新增获取用户余额接口
2021.6.10|GET /v1/market/ticker|新增|新增实时ticker数据接口
2021.6.10|GET /v1/ticker|新增|新增获取k线数据接口
2021.6.10|GET /v1/depth|新增|新增获取深度数据接口
2021.6.10|GET /v1/trade|新增|新增获取实时成交数据接口

