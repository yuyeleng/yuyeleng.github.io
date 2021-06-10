---
sort: 1 # follow a certain sequence of letters or numbers
---
# Change Log

&nbsp;

Release Time(UTC +8)|API|New / Update|Description |
------------- | ------------- |  ------------- | ------------- |
2021.6.10|GET /v1/common/symbols |Add|Add Get all trade pairs
2021.6.10|POST /v1/order/place|Add|Add Place order
2021.6.10|POST /v1/order/cancel|Add|Add Order Cancel，Note:Cancel order requests is under asynchronous pattern,call interface /v1/order/detailById is required for order status query.
2021.6.10|GET /v1/order/detailById|Add|Add Order Details 
2021.6.10|GET /v1/order/counterpartiesById|Add|Add Transaction details
2021.6.10|GET /v1/order/entrust|Add|Add Obtain order list
2021.6.10|GET /v1/order/matchresults|Add|Add Current&History Transaction Record
2021.6.10|POST /v1/order/batchCancelOrders|Add|Add Batch Cancels
2021.6.10|POST /v1/order/batchCancelOpenOrders|Add|Add Batch Cancels(OpenOrders)
2021.6.10|POST /v1/order/batchOrders|Add|Add Batch Orders，API Key Access：Trading,Maximum 10 orders for a single batch
2021.6.10|GET /v1/balance|Add|Add Obtain User Balance
2021.6.10|GET /v1/market/ticker|Add|Add Real-time Ticker data
2021.6.10|GET /v1/ticker|Add|Add Obtain kline data
2021.6.10|GET /v1/depth|Add|Add Obtain Deep Data
2021.6.10|GET /v1/trade|Add|Add Obtain real-time data

