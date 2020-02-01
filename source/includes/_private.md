# Private REST Endpoints

## Scope of the Key

```shell
import jwt # pip install PyJWT
import requests
```


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
