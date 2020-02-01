# Public REST Endpoints

**The base URL for all REST API is:**
**`https://api.oceanex.cc/v1`**

This API gets a list of available markets for trading. (`get` method supported)



## Markets
This API gets a list of available markets for trading.

Rate limit: `3000 req/min`

**HTTP REQUEST**

**GET /markets**



```python
import requests

method_url = base_url + "/markets"
r = requests.post(method_url)
```

```shell
curl -X POST \
  https://api.oceanex.cc/v1/markets
```
