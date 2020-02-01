# Public REST Endpoints

**The base URL for all REST API is:**
**`https://api.oceanex.cc/v1`**

This API gets a list of available markets for trading. (`get` method supported)



## Markets
This API gets a list of available markets for trading.

Rate limit: `3000 req/min`

**HTTP REQUEST**

**GET /markets**




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
