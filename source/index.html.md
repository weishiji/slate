---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python
  - shell: cURL

toc_footers:
  - <a href="https://oceanex.pro">Documentation Powered by OceanEx</a>

search: true
---

# Introduction

Welcome to the OceanEx trading API! You can use our API to access OceanEx endpoints, which can get information and place orders on various markets. Some API supports both `get` and `post` method, but we strongly recommend only to use `post` method for all API requests.

Currently, we only have language bindings in Python. Other languages like Shell, Ruby, and JavaScript are coming soon. You can view code examples in the dark area to the right.

# Public REST Endpoints

The base URL for all REST API is: `https://api.oceanex.cc/v1`.

```python
base_url = "https://api.oceanex.cc/v1"
```
```shell
API endpoint:
https://api.oceanex.cc/v1
```

## Markets `post`
This API gets a list of available markets for trading. (`get` method supported)

```python
import requests

method_url = base_url + "/markets"
r = requests.post(method_url)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/markets
```
> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": [
    {"id": "btcusdt", "name": "BTC/USDT"},
    {"id": "ethusdt", "name": "ETH/USDT"},
    {"id": "vetusdt", "name": "VET/USDT"},
    {"id": "ethbtc", "name": "ETH/BTC"},
    {"id": "vetbtc", "name": "VET/BTC"},
    {"id": "veteth", "name": "VET/ETH"},
    {"id": "vthovet", "name": "VTHO/VET"},
    {"id": "shavet", "name": "SHA/VET"},
    {"id": "shabtc", "name": "SHA/BTC"}
  ]
}
```

Rate limit: `3000 req/min`

##  Ticker `post`
This API gets ticker of specific market.  The snapshot includes the last trade (tick), best bid/ask and volume. (`get` method supported)

Parameters:

| Name | Location | Description | Required | Schema |
|----------|----------|--------------------------------------------------------|--------|-------|
| market | path | The market you want information about. You can find the list of valid markets by calling the /markets endpoint. | Yes | string |

```python
import requests

market = "veteth"
method_url = base_url + "/tickers/{}".format(str(market))
r = requests.post(method_url, timeout=5)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/tickers/veteth
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": {
    "at": 1548123813,
    "ticker": {
      "buy": "0.0000353",
      "sell": "0.00003532",
      "low": "0.00003426",
      "high": "0.00003572",
      "last": "0.00003531",
      "vol": "25430185.0"
    }
  }
}
```

Rate limit: `3000 req/min`


##  Multiple Tickers `post`
This API gets tickers of a list of markets.  The snapshot includes the last trade (tick), best bid/ask and volume. (`get` method supported)

Parameters:

| Name | Location | Description | Required | Schema |
|----------|----------|--------------------------------------------------------|--------|-------|
| markets | query | A dictionary of markets you want information about. | Yes | a dictionary |

```python
import requests

method_url = base_url + "/tickers_multi"
markets = ["veteth", "ethusdt"]
data = {
  "markets[]": markets
}
r = requests.post(method_url, data=data)
```

```shell
curl -X POST -g \
  'https://api.oceanex.cc/v1/tickers_multi?markets[]=veteth&markets[]=ethusdt'
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": [
    {
      "at": 1548123910,
      "market": "veteth",
      "ticker": {
        "buy": "0.00003524",
        "sell": "0.00003526",
        "low": "0.00003426",
        "high": "0.00003572",
        "last": "0.00003526",
        "vol": "25421706.0"
      }
    },
    {
      "at": 1548123910,
      "market": "ethusdt",
      "ticker": {
        "buy": "116.28",
        "sell": "116.92",
        "low": "113.94",
        "high": "117.74",
        "last": "116.63",
        "vol": "73.80781"
      }
    }
  ]
}
```

## All Tickers `post`

This API gets tickers of all the markets. This snapshot include the last trade (tick), best bid/ask, volume and etc.

```python
import requests

method_url = base_url + "/tickers"
r = requests.post(method_url)
```

```shell
curl -X POST 'https://api.oceanex.cc/v1/tickers'
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": {
    {
      "vetusdt": {
        "name":"VET/USDT",
        "display_name":"VET/USDT",
        "base_unit":"vet",
        "quote_unit":"usdt",
        "position":130,
        "low":"0.005299",
        "high":"0.005999",
        "first":"0.005851",
        "last":"0.005682",
        "open":"0.005845",
        "volume":"1650819818.7",
        "sell":"0.005697",
        "buy":"0.005668",
        "at":1563209046,
        "h24_trend":"-2.888395146"
      }
    },
    {
      "ocevet": {
        "name":"OCE/VET",
        "display_name":"OCE/VET",
        "base_unit":"oce",
        "quote_unit":"vet",
        "position":0,
        "low":"0.8599",
        "high":"0.8821",
        "first":"0.8791",
        "last":"0.8755",
        "open":"0.8788",
        "volume":"102124237.0",
        "sell":"0.8778",
        "buy":"0.8755",
        "at":1563209046,
        "h24_trend":"-0.409509726"
      }
    },
  }
}
```

Rate limit: `3000 req/min`


## Order Book `post`
This API gets the order book of a specified market. Both asks and bids are sorted from highest price to lowest. (`get` method supported)

Parameters:

| Name | Location | Description | Required | Schema |
|----------|----------|--------------------------------------------------------|--------|-------|
| market | query | The market you want information about. You can find the list of valid markets by calling the /markets endpoint. | Yes | string |
| limit | query | Limit the number of returned price levels. Default to 300. | No | integer |

```python
import requests

market = "veteth"
limit = 5
method_url = base_url + "/order_book"
data = {
    "market": market,
    "limit": limit
}
r = requests.post(method_url, data=data)
```

```shell
curl -X POST \
  'https://api.oceanex.cc/v1/order_book?market=veteth&limit=5'
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": {
    "timestamp": 1548123910,
    "asks": [
      ["0.8", "879.0"],
      ["0.1", "100.0"],
      ["0.091", "2548.0"],
      ["0.08", "500.0"]
    ],
    "bids": [
      ["0.00003537", "217.0"],
      ["0.0000353", "248.0"],
      ["0.00003525", "48643.0"],
      ["0.00003524", "246.0"]
    ]
  }
}
```

Rate limit: `3000 req/min`

##  Multiple Order Books `post`
This API gets the order books of a list of specified markets. Both asks and bids are sorted from highest price to lowest. (`get` method supported)

Parameters:

| Name | Location | Description | Required | Schema |
|----------|----------|--------------------------------------------------------|--------|-------|
| markets | query | A list of markets you want information about. | Yes | a list of string |
| limit | query | Limit the number of returned price levels. Apply to all markets. Default to 300. | No | integer |

```python
import requests

method_url = base_url + "/order_book/multi"
markets = ["veteth", "ethusdt"]
limit = 5
data = {
  "markets[]": markets,
  "limit": limit
}
r = requests.post(method_url, data=data)
```

```shell
curl -X POST -g  \
 'https://api.oceanex.cc/v1/order_book/multi?markets[]=vetusdt&markets[]=ethusdt&limit=1'
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": [
    {
      "market": "veteth",
      "timestamp": 1539819839,
      "asks": [
        ["0.8", "879.0"],
        ["0.1", "100.0"],
        ["0.091", "2548.0"],
        ["0.08", "500.0"]
      ],
      "bids": [
        ["0.00003537", "217.0"],
        ["0.0000353", "248.0"],
        ["0.00003525", "48643.0"],
        ["0.00003524", "246.0"]
      ]
    },
    {
      "market": "ethusdt",
      "timestamp": 1539819839,
      "asks": [
        ["127.79", "0.56954"],
        ["124.43", "0.50626"],
        ["122.7", "0.38205"],
        ["122.26", "0.3396"]
      ],
      "bids": [
        ["116.68", "0.55"],
        ["116.57", "0.2547"],
        ["116.48", "0.3797"],
        ["116.26", "1.8537"]
      ]
    }
  ]
}
```

Rate limit: `3000 req/min`


## Trades `post`
This API gets recent trades on market. Each trade is included only once. Trades are sorted in reverse creation order. (`get` method supported)

Parameters:

| Name      | Location | Description                                                                                                                | Required | Schema  |
|-----------|----------|----------------------------------------------------------------------------------------------------------------------------|----------|---------|
| market    | query    | The market you want information about. You can find the list of valid markets by calling the /markets endpoint.            | Yes      | string  |
| limit     | query    | Limit the number of returned trades. Default to 50.                                                                        | No       | integer |
| start     | query    | Query start timestamp. If set only trades created after the start time will be returned | No | integer
| end       | query    | Query end timestamp. If set only trades before the end time will be returned | No | interger
| from      | query    | Trade id. If set, only trades created after the trade will be returned.                                                    | No       | integer |
| to        | query    | Trade id. If set, only trades created before the trade will be returned.                                                   | No       | integer |
| order_by  | query    | If set, returned trades will be sorted in specific order. Choose from "asc" or "desc". Default to "desc".                  | No       | string  |

```python
import requests

method_url = base_url + "/trades"
market = "veteth"
limit = 10
start_time = 1547451655
end_time = 1547778117
from_id = 1000
to_id = 1200000
data = {
    "market": market,
    "limit": limit,
    "start": start_time,
    "end": end_time,
    "from": from_id,
    "to": to_id
}
r = requests.post(method_url, data=data)
```

```shell
curl -X POST \
  'https://api.oceanex.cc/v1/trades?market=veteth&limit=1&start=1555693743&end=1556693743&from=1000&to=1200000'
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": [
    {
      "id": 1018348,
      "price": "0.00003526",
      "volume": "6953.0",
      "funds": "0.24516278",
      "market": "veteth",
      "created_at": "2019-01-18T02:21:57Z",
      "created_on": 1547778117,
      "side": null
    },
    {
      "id": 1018342,
      "price": "0.00003526",
      "volume": "6263.0",
      "funds": "0.22083338",
      "market": "veteth",
      "created_at": "2019-01-14T07:40:55Z",
      "created_on": 1547451655,
      "side": null
    }
  ]
}
```

Rate limit: `3000 req/min`

## K Line `post`
This API gets K-line data, including open-high-low-close info. (`get` method supported)

Parameters:

| Name    | Location | Description                                                                                                          | Required | Schema|
|---------|----------|----------------------------------------------------------------------------------------------------------------------|----------|-------|
| market  | query    | The market you want information about                                                                                | Yes      |String |
| limit   | query    | Limit the number of returned data points, default to 30.                                                             | No       |Integer|
| period  | query    | Time period of K line, default to 1. You can choose between 1, 5, 15, 30, 60, 120, 240, 360, 720, 1440, 4320, 10080  | No       |Integer|
|timestamp| query    | An integer represents the seconds elapsed since Unix epoch. If set, only k-line data after that time will be returned| No       |Integer|

```python
import requests
method_url = base_url + "/k"
data = {
  'market': 'btcusdt',
  'limit': 2,
  'period': 5,
  'timestamp': 1559232000,
}
r = requests.post(method_url, data=data)
```

```shell
curl -X POST \
 'https://api.oceanex.cc/v1/k?market=btcusdt&limit=2&period=5&timestamp=1559232000'
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": [
    [1559232000,8889.22,9028.52,8889.22,9028.52,0.3121],
    [1559232300,9052.86,9052.86,8971.44,8996.5,0.1469]
  ]
}
```

Rate limit: `3000 req/min`

## Trading Fees `post`
This API gets trading fees for markets. (`get` method supported)

```python
import requests

method_url = base_url + "/fees/trading"
r = requests.post(method_url)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/fees/trading
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": [
    {
      "market": "btcusdt",
      "ask_fee": {"type": "relative", "value": "0.001"},
      "bid_fee": {"type": "relative", "value": "0.001"}
    },
    {
      "market": "ethusdt",
      "ask_fee": {"type": "relative", "value": "0.001"},
      "bid_fee": {"type": "relative", "value": "0.001"}
    },
    {
      "market": "vetusdt",
      "ask_fee": {"type": "relative", "value": "0.0005"},
      "bid_fee": {"type": "relative", "value": "0.0005"}
    },
    {
      "market": "ethbtc",
      "ask_fee": {"type": "relative", "value": "0.001"},
      "bid_fee": {"type": "relative", "value": "0.001"}
    },
    {
      "market": "vetbtc",
      "ask_fee": {"type": "relative", "value": "0.0005"},
      "bid_fee": {"type": "relative", "value": "0.0005"}
    },
    {
      "market": "veteth",
      "ask_fee": {"type": "relative", "value": "0.0005"},
      "bid_fee": {"type": "relative", "value": "0.0005"}
    },
    {
      "market": "vthovet",
      "ask_fee": {"type": "relative", "value": "0.0005"},
      "bid_fee": {"type": "relative", "value": "0.0005"}
    },
    {
      "market": "shavet",
      "ask_fee": {"type": "relative", "value": "0.0005"},
      "bid_fee": {"type": "relative", "value": "0.0005"}
    },
    {
      "market": "shabtc",
      "ask_fee": {"type": "relative", "value": "0.001"},
      "bid_fee": {"type": "relative", "value": "0.001"}
    }
  ]
}
```

Rate limit: `3000 req/min`

## API Server Time `post`
This API gets the API server"s time (in seconds since Unix epoch). (`get` method supported)

```python
import requests

method_url = base_url + "/timestamp"
r = requests.post(method_url)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/timestamp
```

> The above command returns JSON structured like this:

```json
{
  "code": 0,
  "message": "Operation successful",
  "data": 1548127147
}
```

Rate limit: `3000 req/min`

# REST Authentication

Users need to provide `uid`, `apikey_id`, and `user_private_key` to authenticate. In addition, users need to add their IP address to the whitelist. All authenticated API will call the function `construct_request_body` by feeding three arguments. The first and second arguments specify the method (e.g. get or post) and the corresponding URL. The last argument includes all parameters needed for the particular API.

<blockquote class="lang-specific python">
<p>To generate key pairs, use the following code in terminal.</p>
<p>Generate an RSA private key, of size 2048, and output it to a file named key.pem:
</p>
</blockquote>

```python
$ openssl genrsa -out key.pem 2048
```

<blockquote class="lang-specific python">
<p>Extract the public key from the key pair:</p>
</blockquote>

```python
$ openssl rsa -in key.pem -outform PEM -pubout -out public.pem
$ cat public.pem

# public key format
# -----BEGIN PUBLIC KEY-----
# MIIBI***9XNmrQIDAQAB
# -----END PUBLIC KEY-----
```

#### Upload `user_public_key`
a. Get public key by instructions
b. Access OceanEx dashboard and head to 'Setting' page
<p><img src="/images/Setting.png" style="max-width:100%;height:auto"><p>

c. Click `Apikey` tab. (User must pass verification to using trading API and link Google Authenticator)

d. Set lable, public key, permissions, and trusted IPs. Click `Add a new key` button to submit
<p><img src="/images/API.png" style="max-width:100%;height:auto"><p>

e. After submitting, using `uid` and `apikey_id` (key Id) to send API request
<p><img src="/images/Key.png" style="max-width:100%;height:auto"><p>

<aside class="notice">
You must replace <code>uid</code>, <code>apikey_id</code>, and <code>user_private_key</code> with your personal information.
</aside>

## Using Python

API request via python demo

```python
import jwt # pip install PyJWT
import requests

uid = "ID6A9EAFD896"
apikey_id = "KIDF4A8334E1E"
with open("key.pem", "rb") as private_file:
  user_private_key = private_file.read()

def construct_request_body(data):
  pay_load = {
     "uid": uid,
     "apikey_id": apikey_id, #optional
     "data": data
  }

  JWT_TOKEN = jwt.encode(pay_load, user_private_key, algorithm="RS256")
  request_body = {
      "user_jwt": JWT_TOKEN
  }
  return request_body

method_url = base_url + "/key"
data = {}
request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
print(r.text())
```

## Using Curl Command

<blockquote class="lang-specific shell">
<p>Generate JWT_Token using bash</p>
</blockquote>

```shell

set -o pipefail

header_template='{
    "typ": "JWT",
    "alg": "RS256"
}'

b64enc() { openssl enc -base64 -A | tr '+/' '-_' | tr -d '='; }
json() { jq -c . | LC_CTYPE=C tr -d '\n'; }
rs_sign() { openssl dgst -binary -sha"${1}" -sign <(printf '%s\n' "$2"); }

sign() {
        local algo payload header sig secret=$3
        algo=${1:-RS256}; algo=${algo^^}
        header=${header_template}
        payload=${2:-$payload}
        signed_content="$(json <<<"$header" | b64enc).$(json <<<"$payload" | b64enc)"
        sig=$(printf %s "$signed_content" | rs_sign "${algo#RS}" "$secret" | b64enc)
        printf '%s.%s\n' "${signed_content}" "${sig}"
}


payload='{
    "uid": "IDXXXXXXXXX",
    "data": {

    }
}'
rsa_secret=$(cat key.pem)
sign rs256 "$payload" "$rsa_secret"
#JWT_token printed
```
<blockquote class="lang-specific shell">
<p>Use generated JWT_Token to send Curl command</p>
</blockquote>

```shell
$ curl -X POST https://api.oceanex.cc/v1/API -F user_jwt= JWT_TOKEN
```
Private API needs encoded JWT_Token to ensure security. Use bash commands to generate JWT_Token first. Then, send curl command with it.

Before use bash commands, ensure `jq`, `openssl`, and `base64` installed

# Private REST Endpoints

## Scope of the Key `post`
This API gets the scope of your key. (`get` method supported)

```python
import requests

method_url = base_url + "/key"
data = {}
request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/key?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": [
    {
      "scopes": ["query","sell/buy"],
      "apikey_id": "KIDF4A8334***",
      "created_at": "2018-01-01T00:00:00.000Z",
      "ips": ["35.237.103.8", "35.245.144.195"],
      "state": "1",
      "pub_key": "*******",
      "alias_name": "test1",
      "id": "21"
    }
  ]
}

```
Response Details from the `data` field.

| Key        | Description                                                                                   | Schema |
|------------|-----------------------------------------------------------------------------------------------|--------|
| scopes     | The scope of your key.  There are three level: "query", "sell/buy", and "withdraws/deposits". | string |
| apikey_id  | The id of apikey
| created_at | The created time of API key
| state      | The indicator of whether the key is still enabled. 1: enabled, 0: disabled.                   | binary |
| ips        | IPs in the whitelist.,Separated by ",".                                                       | string |
| pub_key    | The public key
| alias_name | The alias of your key.                                                                        | string |
| id         | apikey ID

Rate limit: `3000 req/min`

## Account Info `post`
This API gets the information about your account. (`get` method supported)

```python
import requests

method_url = base_url + "/members/me"
data = {}
request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/members/me?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "code":0,
  "message":"Operation successful",
  "data":{
    "accounts": [
      {
        "currency":"vet",
        "balance":"1967.031",
        "locked":"0.0"
      },
      {
        "currency":"btc",
        "balance":"0.0",
        "locked":"0.0"
      },
      {
        "currency":"eth",
        "balance":"0.0",
        "locked":"0.0"
      }
    ]
  }
}

```

Rate limit: `3000 req/min`

## New Order `post`
This API submits a new order.

Parameters:

| Name | Description | Required | Schema |
|--------|------------------------------------------------------------------------------------------|--------|------------|
| market | The market you want information about. You can find the list of valid markets by calling the /markets endpoint. | Yes | string |
| side | Either "sell" or "buy". | Yes | string |
| volume | The amount user want to sell or buy. | Yes | string/float |
| price | Price for each unit. Price is required for "limit" order only. No need for "market" order. | Yes/No | string/float |
| ord_type |  Default "limit". Supported "limit",  "market", "stop_limit" order. "stop_market" will be available later | No | string |
| stop_price | Used by "stop_limit order" as trigger requirement. Required if "ord_type" is "stop_limit" | Yes/No | string/float |
| trigger_condition | Accepted value: "gte" (Greater or equal), "lte" (Less or equal). Required if "ord_type" is "stop" order  |Yes/No| string|

```python
import requests

method_url = base_url + "/orders"
#eg: Limit Order
data = {
    "market": "veteth",
    "side": "buy",
    "volume": 0.75,
    "price": 1000,
    "ord_type": "limit"
}
"""
#eg: Market Order
data = {
    "market": "veteth",
    "side": "buy",
    "volume": 20,
    "ord_type" : "market"
}

# eg:  Stop Limit Order
data = {
    "market": "veteth",
    "side": "buy",
    "volume": 20,
    "price": 1000,
    "ord_type": "stop_limit",
    "stop_price": 900,
    "trigger_condition": "lte"
}
"""

request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/orders?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": {
    "created_at": "2019-01-18T00:38:17Z",
    "trades_count": 0,
    "remaining_volume": "0.75",
    "price": "1000.0",
    "created_on": "1547771897",
    "side": "buy",
    "volume": "0.75",
    "state": "wait",
    "ord_type": "limit",
    "avg_price": "0.0",
    "executed_volume": "0.0",
    "id": 473791,
    "market": "veteth"
  }
}
```

Response Details from the `data` field.  These fields are commonly shared accross multiple APIs involving `/orders`, `/order/delete`, `/orders/clear`, `/orders/multi`, etc.
For orders, we have the following states:[wait wait_trigger cancel done cancelling].
Order in wait(for normal order only) or wait_trigger(for stop-limit order only) state means the order is placed and not yet triggered(for stop-limit order only), filled(for normal order only), cancelling(all types of orders) or cancelled(all types of orders). Order in cancel state means the order is cancelled by the end user or by system. Order in cancelling state means the order is cancelling and not yet settled. Furthermore, order in cancelling state will be removed from orderbook and thus will not be further matched. However, the already matched portion will still be granted. Order in done state means the order is fully filled.

|        Key       |                                                 Description                                                 |  Schema |
|----------------|-----------------------------------------------------------------------------------------------------------|-------|
| remaining_volume |                                      The remaining volume of the order.                                     |  string |
|   trades_count   |                               How many trades of the order has been executed.                               | integer |
|    created_at    |                                      The time when the order created.                                       |  string |
|       side       |                                           Either “buy” or “sell”.                                           |  string |
|        id        |                                                  Order ID.                                                  | integer |
|      volume      |                                      The original volume of the order.                                      |  string |
|     ord_type     |                                                  "limit"                                         |  string |
|       price      |                      The price the order was issued at (can be null for market orders).                     |  string |
|    created_on    |  The timestamp when the order created | integer
|     avg_price    | The average price at which this order as been executed so far. 0 if the order has not been executed at all. |  string |
|       state      |                                         "wait", "wait_trigger", "cancel", "done, "cancelling"               |  string |
|  executed_volume |                                   How much of the order has been executed.                                  |  string |
|      market      |                                    The market name the order belongs to.                                    |  string |

Rate limit: `1200 order/min`

<aside class="notice">
To sucessfully create a new order, order amount (price * volume) must be greater than the market minimum_trading_amount.
</aside>

| Market  |  Minimum_trading_amount|
|---------|----------------------- |
| btcusdt |    10.0000000000000000 |
| ethbtc  |     0.0010000000000000 |
| ethusdt |    10.0000000000000000 |
| oceusdt |     0.0000000000000000 |
| ocevet  |   100.0000000000000000 |
| shabtc  |     0.0010000000000000 |
| shavet  |   100.0000000000000000 |
| ticvet  |   100.0000000000000000 |
| vetbtc  |     0.0010000000000000 |
| veteth  |     0.0100000000000000 |
| vetusdt |    10.0000000000000000 |
| vthovet |   100.0000000000000000 |
| vthousdt|     1.0000000000000000 |

## Multiple New Orders `post`
This API submits several new orders at once.

Parameters:

| Name | Description | Required | Schema |
|------|---------------------------------------------------------------------------------------------------------------|--------|----------|
| market | The market you want information about. You can find the list of valid markets by calling the /markets endpoint. | Yes | string |
| orders | A list of orders. Each order a dictionary which has the same entries as the API for creating a single order except without "market". | Yes | a list of dictionary |

```python
import requests

method_url = base_url + "/orders/multi"
orders = [{"side":"buy", "volume":.2, "price":1001},
          {"side":"sell", "volume":.2, "price":1002}]
data = {
    "market": "veteth",
    "orders": orders
}
request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/orders/multi?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": [
    {
      "created_at": "2019-01-18T00:38:18Z",
      "trades_count": 0,
      "remaining_volume": "0.2",
      "price": "1001.0",
      "created_on": "1547771898",
      "side": "buy",
      "volume": "0.2",
      "state": "wait",
      "ord_type": "limit",
      "avg_price": "0.0",
      "executed_volume": "0.0",
      "id": 473797,
      "market": "veteth"
    },
    {
      "created_at": "2019-01-18T00:38:18Z",
      "trades_count": 0,
      "remaining_volume": "0.2",
      "price": "1002.0",
      "created_on": "1547771898",
      "side": "sell",
      "volume": "0.2",
      "state": "wait",
      "ord_type": "limit",
      "avg_price": "0.0",
      "executed_volume": "0.0",
      "id": 473798,
      "market": "veteth"
    }
  ]
}

```

Rate limit: `1200 order/min`

## Order Status `get`

This API gets information of a list of specified order.

Parameters:

| Name  | Description | Required | Schema |
|----|----------------|--------|-------|
| ids | A list of unique order id. | Yes | a list of integer |

```python
import requests

method_url = base_url + "/orders"
data = {
    "ids": [473797, 473798]
}
request_body = construct_request_body(data)

r = requests.get(method_url, data=request_body)
```

```shell
curl -X GET \
  https://api.oceanex.cc/v1/orders?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": [
    {
      "created_at": "2019-01-18T00:38:18Z",
      "trades_count": 0,
      "remaining_volume": "0.2",
      "price": "1001.0",
      "created_on": "1547771898",
      "side": "buy",
      "volume": "0.2",
      "state": "wait",
      "ord_type": "limit",
      "avg_price": "0.0",
      "executed_volume": "0.0",
      "id": 473797,
      "market": "veteth"
    },
    {
      "created_at": "2019-01-18T00:38:18Z",
      "trades_count": 0,
      "remaining_volume": "0.2",
      "price": "1002.0",
      "created_on": "1547771898",
      "side": "sell",
      "volume": "0.2",
      "state": "wait",
      "ord_type": "limit",
      "avg_price": "0.0",
      "executed_volume": "0.0",
      "id": 473798,
      "market": "veteth"
    }
  ]
}

```

Rate limit: `3000 req/min`

## Order Status with Filters `post`

This API gets your orders, results is paginated. You are able to filter the results based on various options. (`get` method supported)

Parameters:

|   Name   |                                                   Description                                                   | Required |  Schema |
|--------|---------------------------------------------------------------------------------------------------------------|--------|-------|
|  market  | The market you want information about. You can find the list of valid markets by calling the /markets endpoint. |    Yes   |  string |
|  states  |                Filter order by a list of states. Choose from "wait", "done" or "cancel". Default to "wait".                |    No    |  a list of string |
|   limit  |                               Limit the number of returned orders per page, default to 100.                              |    No    | integer |
|   page   |                                      Specify the page of paginated results.                                     |    No    | integer |
| from     | Trade id. If set, only trades created after the trade (including the set id) will be returned.                                                    | No       | integer |
| to       | Trade id. If set, only trades created before the trade (including the set id) will be returned.                                                   | No       | integer |
| order_by |    If set, returned orders will be sorted by their ids in a specific order.  Choose from "asc" or "desc".  Default to "desc".   |    No    |  string |
| need_price | If set, returned orders will include price. | NO | boolean
```python
import requests

method_url = base_url + "/orders/filter"
data = {
    "market": "veteth",
    "states": ["cancel", "done"], # wait, done or cancel
    "limit": 3,
    "page": 1,
    "from": 50,
    "to": 51,
    "order_by": "desc",
    "need_price": "True"
}
request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/orders/filter?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": [
    {
      "num_of_orders": 0,
      "state": "cancel",
      "orders": []
    },
    {
      "state": "done",
      "num_of_orders": 2,
      "orders": [
        {
          "created_at": "2018-09-28T12:40:51.000Z",
          "trades_count": 1,
          "remaining_volume": "0.0",
          "market_id": "veteth",
          "price": "1000",
          "created_on": "1538138451",
          "side": "buy",
          "volume": "0.5",
          "state": "done",
          "ord_type": "limit",
          "market_name": "VET/ETH",
          "avg_price": "11.0",
          "executed_volume": "0.5",
          "market_display_name": "VET/ETH",
          "id": 50
        },
        {
          "created_at": "2018-09-28T12:40:52.000Z",
          "trades_count": 1,
          "remaining_volume": "0.0",
          "market_id": "veteth",
          "price": "1000",
          "created_on": "1538138452",
          "side": "sell",
          "volume": "0.5",
          "state": "done",
          "ord_type": "limit",
          "market_name": "VET/ETH",
          "avg_price": "11.0",
          "executed_volume": "0.5",
          "market_display_name": "VET/ETH",
          "id": 51
        }
      ]
    }
  ]
}
```

Rate limit: `3000 req/min`


## Cancel Order `post`
This API cancels an order.

Parameters:

| Name  | Description | Required | Schema |
|----|----------------|--------|-------|
| id | Unique order id. | Yes | integer |

```python
import requests

method_url = base_url + "/order/delete"
data = {
    "id": 473797
}
request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/order/delete?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": {
    "created_at": "2019-01-18T00:38:18Z",
    "trades_count": 1,
    "remaining_volume": "0.0",
    "price": "1001.0",
    "created_on": "1547771898",
    "side": "buy",
    "volume": "0.2",
    "state": "done",
    "ord_type": "limit",
    "avg_price": "1001.0",
    "executed_volume": "0.2",
    "id": 473797,
    "market": "veteth"
  }
}

```

Rate limit: `3000 req/min`

## Cancel Multiple Orders `post`

This API cancels a list of orders.

Parameters:

| Name  | Description | Required | Schema |
|----|----------------|--------|-------|
| ids | A list of unique order id. | Yes | a list of integer |

```python
import requests

method_url = base_url + "/order/delete/multi"
data = {
    "ids": [473797, 473798]
}
request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/order/delete/multi?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": [
    {
      "created_at": "2019-01-18T00:38:18Z",
      "trades_count": 1,
      "remaining_volume": "0.0",
      "price": "1001.0",
      "created_on": "1547771898",
      "side": "buy",
      "volume": "0.2",
      "state": "done",
      "ord_type": "limit",
      "avg_price": "1001.0",
      "executed_volume": "0.2",
      "id": 473797,
      "market": "veteth"
    },
    {
      "created_at": "2019-01-18T00:38:18Z",
      "trades_count": 0,
      "remaining_volume": "0.2",
      "price": "1002.0",
      "created_on": "1547771898",
      "side": "sell",
      "volume": "0.2",
      "state": "wait",
      "ord_type": "limit",
      "avg_price": "0.0",
      "executed_volume": "0.0",
      "id": 473798,
      "market": "veteth"
    }
  ]
}
```

Rate limit: `3000 req/min`

## Cancel All Orders `post`
This API cancels all orders.

Parameters:

| Name | Description | Required | Schema |
|----|----------------------------------------------------------------------------------------------------|--------|------|
| side | Either "buy" or "sell". If present, only sell orders (asks) or buy orders (bids) will be canncelled. | No | string |

```python
import requests

method_url = base_url + "/orders/clear"
data = {}
request_body = construct_request_body(data)

r = requests.post(base_url + method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/orders/clear?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
  "message": "Operation successful",
  "code": 0,
  "data": [
    {
      "created_at": "2018-10-18T00:14:33Z",
      "trades_count": 1,
      "remaining_volume": "0.12873111",
      "price": "950.47527434",
      "created_on": "1539821673",
      "side": "buy",
      "volume": "0.57765189",
      "state": "wait",
      "ord_type": "limit",
      "avg_price": "950.47527434",
      "executed_volume": "0.44892078",
      "id": 469417,
      "market": "veteth"
    },
    {
      "created_at": "2018-10-18T00:38:19Z",
      "trades_count": 1,
      "remaining_volume": "0.2",
      "price": "963.18955699",
      "created_on": "1539823099",
      "side": "buy",
      "volume": "0.94792349",
      "state": "wait",
      "ord_type": "limit",
      "avg_price": "963.18955699",
      "executed_volume": "0.74792349",
      "id": 473803,
      "market": "veteth"
    }
  ]
}
```

Rate limit: `3000 req/min`

# WAPI Endpoints (to be released)

## Special Withdraw `post`

```python
import requests

method_url = base_url + "/withdraws/special/new"
data = {
    "rid": "0x7781a6C9B25B529037a85ad637669D848a1F9796",
    "currency": "vet",
    "amount": 10,
    "memo": "A note about the withdraw"
}

request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/withdraws/special/new?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
    "code": 0,
    "message": "Operation successful",
    "data": {
        "id":1372,
        "currency":"VET",
        "chain_name":"",
        "type":"coin",
        "amount":"10.0",
        "fee":"1.0",
        "blockchain_txid":null,
        "rid":"0x7781a6C9B25B529037a85ad637669D848a1F9796",
        "is_combination_address":false,
        "address":"0x7781a6C9B25B529037a85ad637669D848a1F9796",
        "memo":"A note about the withdraw",
        "state":"fund_requesting",
        "created_at":"2019-11-06T14:58:06Z",
        "updated_at":"2019-11-06T14:58:06Z",
        "completed_at":null,
        "created_on":1573052286,
        "updated_on":1573052286,
        "completed_on":null,
        "done_at":null
    }
}
```

Make a withdraw while bypassing Google authentication and email verification.
Requires address (rid) to be whitelisted.

Parameters:

| Name | Description | Required | Schema |
|----|----------------------------------------------------------------------------------------------------|--------|------|
| tid | The shared transaction ID. Must not exceed 64 characters. The system will generate one automatically unless supplied. | No | string |
| rid | Withdraw destination address or beneficiary ID | Yes | string |
| currency | Currency ID| Yes | string |
| chain_name | The chain name of the currency (if applicable) | No | string|
| amount | Amount to withdraw | Yes | float |
| memo | Memo of withdraw | No | string |


### Transaction Response Parameters:

| Key                   | Description                       | Schema     |
|-----------------------|-----------------------------------|------------|
| id                    | Transaction id of OceanEx         | Interger   |
| currency              | Currency name                     | String     |
|chain_name             | Blockchain name                   | String     |
| type                  | Withdraw currency type            | String     |
| amount                | Withdraw amount                   | Float      |
| fee                   | Fee charged                       | Float      |
|blockchain_txid        | Blockchain Transaction Id         | String     |
| rid                   | Withdraw destination address or beneficiary ID   | String     |
| is_combination_address| Whether address is combination address | Boolean |
| address               | Destination address                 | String |
| memo                  | Transaction Memo                    | String |
| state                 | Transaction state                   | String |
| created_at            | Transaction created time(ISO8601)   | Datetime |
| updated_at            | Transaction updated time(ISO8601)   | Datetime |
| completed_at          | Transaction completed time(ISO8601) | Datetime |
| created_on            | Transaction created time            | Timestamp|
| updated_on            | Transaction updated time            | Timestamp|
| completed_on          | Transaction completed time          | Timestamp|
| done_at               | Transaction completed time(ISO8601) | Datetime |

Rate limit: `3000 req/min`

## Deposit Address `post`

```python
import requests

method_url = base_url + "/deposit_address"
data = {
    "currency": "vet"
}

request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/deposit_address?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
    "code": 0,
    "message": "Operation successful",
    "data": {
        "currency": "vet",
        "address": "0x12B937eF96553948fAE98e9Ba1e3c027Da3ae548"
    }
}
```

Get your deposit address for a currency.

Parameters:

| Name     | Description|Required| Schema |
|----------|------------|--------|--------|
| currency | Currency ID| Yes    | string |

Rate limit: `3000 req/min`

## Deposit Addresses `post`

```python
import requests

method_url = base_url + "/deposit_addresses"
data = {
    "currency": "eth"
}

request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/deposit_addresses?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
    "code": 0,
    "message": "Operation successful",
    "data": {
        "data": {
          "currency_id": "vet",
          "display_name": "VET",
          "num_of_resources": 1,
          "resources": [
            {
              "chain_name": "",
              "currency_id": "vet",
              "address": "0x12B937eF96553948fAE98e9Ba1e3c027Da3ae548",
              "memo": "",
              "deposit_status": "enabled"
            }
          ]
        }
    }
}
```

Get deposit addresses for a currency.

Parameters:

| Name | Description | Required | Schema |
|----|----------------------------------------------------------------------------------------------------|--------|------|
| currency | Currency ID| Yes | string |

## Deposit History `post`

```python
import requests

method_url = base_url + "/deposit_history"
data = {
    "currency": "vet",
    "limit": 5,
    "state": "accepted",
    "page": 1
}

request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/deposit_history?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
    "code": 0,
    "message": "Operation successful",
    "data": {
        "num_of_resources": 6,
        "resources": [
            {
                "id": 1071,
                "currency": "VET",
                "amount": "99.0",
                "fee": "0.0",
                "txid": "0x390b89847b080228bcb10c609291cf73ffdb501a8725e48c8c09b61694d47d5c",
                "is_combination_address": false,
                "address": "0x12B937eF96553948fAE98e9Ba1e3c027Da3ae548",
                "memo": "",
                "created_at": "2019-10-15T16:27:48Z",
                "created_on": 1571156868,
                "confirmations": 7,
                "completed_at": "2019-10-15T16:28:49Z",
                "completed_on": 1571156929,
                "state": "accepted"
            }
        ]
    }
}
```

Get a list of your deposits.

Parameters:

| Name | Description | Required | Schema |
|----|-----------------------|--------|------|
| currency | Currency ID| No | string |
| page | Page number (defaults to 1) | No | Integer |
| limit | Set result limit | No | Integer |
| state | State of deposits | No | String |

### Possible values for state
[submitted, canceled, rejected, reverted, accepted, pending, processing
settled, suspending, suspended, suspend_failed, clearing, clear_failed]

Rate limit: `3000 req/min`

## Withdraw History `post`

```python
import requests

method_url = base_url + "/withdraw_history"
data = {
    "currency": "vet",
    "limit": 3,
    "page": 1
}

request_body = construct_request_body(data)

r = requests.post(method_url, data=request_body)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/withdraw_history?user_jwt= JWT_TOKEN
```

> The above command returns JSON structured like this:

```json
{
    "code": 0,
    "message": "Operation successful",
    "data": {
        "num_of_resources": 1,
        "resources": [
           {
             "id": 1372,
             "currency": "VET",
             "chain_name": "",
             "type": "coin",
             "amount": "10.0",
             "fee": "1.0",
             "blockchain_txid": null,
             "rid": "0x7781a6C9B25B529037a85ad637669D848a1F9796",
             "is_combination_address": false,
             "address": "0x7781a6C9B25B529037a85ad637669D848a1F9796",
             "memo": "",
             "state": "rejected",
             "created_at": "2019-11-06T14:58:06Z",
             "updated_at": "2019-11-06T15:17:36Z",
             "completed_at": "2019-11-06T15:17:36Z",
             "created_on": 1573052286,
             "updated_on": 1573053456,
             "completed_on": 1573053456,
             "done_at": "2019-11-06T15:17:36Z"
           }
        ]
    }
}
```

Get a list of the your withdraws.

Parameters:

| Name | Description | Required | Schema |
|----|----------------------------------------------------------------------------------------------------|--------|------|
| currency | Currency ID | No | string |
| limit |  Number of withdraws per page (defaults to 100, maximum is 1000) | No | Integer |
| page | Page number (defaults to 1) | No | Integer |

Rate limit: `3000 req/min`

# REST Errors

The OceanEx trading API uses the following error codes:

Error Code | Meaning
---------- | -------
-1, -2, -9, 1005 | Server error -- Please be patient.
1003, 9001 | Algorithm error -- Your JWT encoding algorithm is wrong, RS 256 encoding is required.
1004, 9002 | Missing parameters or invalid format in authentication payload.
1001 | JWT error -- Token is not in a valid JWT format.
1002 | Payload cannot be empty.
1006 | The account does not exist.
1007 | Your account has no public key.
1008 | Your key is disabled.
1010 | Token verification failed.
1011 | This IP "xxx.xxx.xxx.xxx" is not allowed.
2001 | Token expired. Authorization failed
2002 | Failed to create order. Error parameter or exceeding request limit
2003 | Failed to cancel order
2004 | Order doesn't exit
2012 | Deposit doesn't exit
3001 | Withdraw amount less than required amount
3002 | Withdraw rid is not whitelisted
9003 | You do not have API "xx:/xx/xxx" permissions, or the API does not exist.

> Common authentication errors include:

> The key is disabled.

```json
{
  "data": {},
  "code": 1008,
  "message": "Your key is disabled"
}
```

> Invalid Key.

```json
{
  "message": "Token verification failed",
  "code": 1010,
  "data": {}
}
```

> IP is not on a white list.

```json
{
  "message": "This IP "22.222.222.222" is not allowed",
  "code": 1011,
  "data": {}
}
```

> The scope is insufficient.

```json
{
  "message": "You do not have API "post:/v2/orders/multi" permissions, or the API does not exist",
  "code": 9003,
  "data": {}
}
```

# Websockets
The base URL for all websockets is: `wss://ws-slanger.oceanex.cc/app`.  To access the websocket services, the URL needs to append more parameters.   Please refer to the python code for the url construction, which makes the final URL as `wsURL = wss://ws-slanger.oceanex.cc/app/03e22cd99036bbfee126ce6b9725?protocol=7&version=4.3.1&flash=false&client=js`.

```python
url = "wss://ws-slanger.oceanex.cc/app"
application_id = "03e22cd99036bbfee126ce6b9725"
ws_params = {"protocol": "7", "client": "js", "version": "4.3.1", "flash": "false"}

# Get WS URL and connect.
params_string = "&".join([key + "=" + value for key, value in ws_params.items()])
wsURL = url + "/" + application_id + "?" + params_string
```
> The above command returns URL as:

```bash
wsURL = "wss://ws-slanger.oceanex.cc/app/03e22cd99036bbfee126ce6b9725?protocol=7&version=4.3.1&flash=false&client=js"
```

## Tickers
If you are already connected and wish to recieve the tickers of all markets, subscribe to the channel of `market-global`, and send a message with the following format.

```python
import json

from websocket import create_connection
ws = create_connection("wss://ws-slanger.oceanex.cc/app/03e22cd99036bbfee126ce6b9725?flash=false&client=js&protocol=7&version=4.3.1")
ticker_subscription = {
  "event": "pusher:subscribe",
  "data":{
    "channel": "market-global"
  }
}
ws.send(json.dumps(ticker_subscription))
print("Sent")
print("Receiving...")
while True:
#Add while loop to extend stream connection time
  result =  ws.recv()
  print("Received '%s'" % result)
ws.close()
```

> The expected return likes this:

```json
{
  "data": {
    "ocevet":{
      "name":"OCE/VET",
      "base_unit":"oce",
      "quote_unit":"vet",
      "low":"10.0",
      "high":"3000.21288263",
      "last":"2919.0837592",
      "open":1590.54056637,
      "volume":"358526.07170156",
      "sell":"2954.5",
      "buy":"2909.5",
      "at":1540521383,
      "h24_trend":"83.053416479242466858993467065"
    },
    "usdeur":{
      "name":"USD/EUR",
      "base_unit":"usd",
      "quote_unit":"eur",
      "low":"0.0",
      "high":"0.0",
      "last":"0.79999999",
      "open":0.79999999,
      "volume":"0.0",
      "sell":"1.09999999",
      "buy":"0.79999999",
      "at":1540521383,
      "h24_trend":"0.0"
    }
  },
  "event": "tickers",
  "channel": "market-global"
}
```

## Order Book
If you wish to receive the order book of a particular market with decimal precision, then subscribe to the channel of `market-<MARKET NAME>-<PRECISION>-global`.  For instance, if you want to subscribe to the market `ocevet` with the precision level at 4, then the channel name becomes `market-ocevet-4-global`.  The range of the precision level is from 1 to 8.  You can send a message with the following format.

```python
import json

from websocket import create_connection
ws = create_connection("wss://ws-slanger.oceanex.cc/app/03e22cd99036bbfee126ce6b9725?flash=false&client=js&protocol=7&version=4.3.1")
market_subscription = {
  "event": "pusher:subscribe",
  "data":{
    "channel": "market-ocevet-6-global"
  }
}
ws.send(json.dumps(market_subscription))
print("Sent")
print("Receiving...")
while True:
#Add while loop to extend stream connection time
  result =  ws.recv()
  print("Received '%s'" % result)
ws.close()
```

> The expected return likes this:

```json
{
  "data":{
    "asks":[
      ["2924.5","0.001"],
      ["2939.0","0.002"],
      ["2953.5","0.003"],
      ["2984.0","0.004"],
      ["2998.5","0.005"]
    ],
    "bids":[
      ["2895.0","0.001"],
      ["2894.5","0.002"],
      ["2866.0","0.003"],
      ["2851.5","0.004"],
      ["2837.5","0.005"]
    ]
  },
  "event": "update",
  "channel": "market-ocevet-6-global"
}
```

## Trades
If you wish to receive the trades of a particular market, then subscribe to the channel of `market-<MARKET NAME>-trade-global`.  For instance, if you want to subscribe to the market `ocevet`, then the channel name becomes `market-ocevet-trade-global`. You can send a message with the following format.

```python
import json

from websocket import create_connection
ws = create_connection("wss://ws-slanger.oceanex.cc/app/03e22cd99036bbfee126ce6b9725?flash=false&client=js&protocol=7&version=4.3.1")
trade_subscription = {
    "event": "pusher:subscribe",
    "data": {
        "channel": "market-ocevet-trade-global"
    }
}
ws.send(json.dumps(trade_subscription))
print("Sent")
print("Receiving...")
while True
#Add while loop to extend stream connection time
  result =  ws.recv()
  print("Received '%s'" % result)
ws.close()
```


> The expected return likes this:

```json
{
  "channel": "market-ocevet-trade-global",
  "event": "trades",
  "data": {
    "trades":[
      {
        "tid":1365629,
        "type":"sell",
        "date":1540523615,
        "price":"2890.77493651",
        "amount":"0.65200712"
      }
    ]
  }
}
```

## Example Code
Please refer the following code to access our websockets.

```python
import websocket
import json

class OceanExWebsocket():
    def __init__(self):
        pass

    def connect(self, endpoint, application_id, ws_params):
        # Get WS URL and connect.
        params_string = "&".join([key + "=" + value for key, value in ws_params.items()])
        params_string = "?" + params_string
        wsURL = endpoint + "/" + application_id + params_string
        print (wsURL)

        self.__connect(wsURL)

    def __connect(self, wsURL):
        #Connect to the websocket
        self.ws = websocket.WebSocketApp(wsURL, on_message=self.__on_message, on_close=self.__on_close,
                                         on_open=self.__on_open, on_error=self.__on_error)
        self.ws.run_forever()

    def __on_message(self, _, message):
        #Handler for parsing WS messages. We just print the message out
        message = json.loads(message)
        print (message)



    def __on_open(self, _):
        print("######## WS Opened ########")

        market_subscription = {"event": "pusher:subscribe",
                               "data": {"channel": "market-ocevet-6-global"}}

        ticker_subscription = {"event": "pusher:subscribe",
                               "data": {"channel": "market-global"}}

        trade_subscription = {"event": "pusher:subscribe",
                               "data": {"channel": "market-ocevet-trade-global"}}

        self.ws.send(json.dumps(ticker_subscription))
        self.ws.send(json.dumps(market_subscription))
        self.ws.send(json.dumps(trade_subscription))


    def __on_close(self, _):
        print("######## WS closed ########")

    def __on_error(self, _, error):
        print("######## WS Error ########")
        print(error)
        self.ws.close()

if __name__ == "__main__":

  url = "wss://ws-slanger.oceanex.cc/app"
  application_id = "03e22cd99036bbfee126ce6b9725"
  ws_params = {"protocol": "7", "client": "js", "version": "4.3.1", "flash": "false"}

  oceanex_ws = OceanExWebsocket()
  oceanex_ws.connect(url, application_id, ws_params)

```
