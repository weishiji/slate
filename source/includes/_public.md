# Public REST Endpoints

## Markets

```shell
curl -X GET \
  https://api.oceanex.cc/v1/markets
```

```python
#   pip install requests
#
# or, with easy-install:
#
#   easy_install requests
import requests

method_url = base_url + "/markets"
r = requests.post(method_url)
```

> Response

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

**The base URL for all REST API is:**
**`https://api.oceanex.cc/v1`**

This API gets a list of available markets for trading. (`get` method supported)




This API gets a list of available markets for trading.

Rate limit: `3000 req/min`

**HTTP REQUEST**

**GET /markets**

## Tickers

```python
import requests

market = "veteth"
method_url = base_url + "/tickers/{}".format(str(market))
r = requests.post(method_url, timeout=5)
```

```shell
curl -X GET \
  https://api.oceanex.cc/v1/tickers/veteth
```

> Response:

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

This API gets ticker of specific market.  The snapshot includes the last trade (tick), best bid/ask and volume. (`get` method supported)

Rate limit: `3000 req/min`

**HTTP REQUEST**

**GET /tickers**


**Parameters:**

| Name | Location | Description | Required | Schema |
|----------|----------|--------------------------------------------------------|--------|-------|
| market | path | The market you want information about. You can find the list of valid markets by calling the /markets endpoint. | Yes | string |

##  Multiple Tickers

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

> Response:

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

This API gets tickers of a list of markets.  The snapshot includes the last trade (tick), best bid/ask and volume. (`get` method supported)

**HTTP REQUEST**

**GET /tickers**

**Parameters:**

| Name | Location | Description | Required | Schema |
|----------|----------|--------------------------------------------------------|--------|-------|
| markets | query | A dictionary of markets you want information about. | Yes | a dictionary |
