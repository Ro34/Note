#### 带参数请求
###### 方法1
```
import requests

url = "http://test/api/auth/me?token=xxx" 

res = requests.get(url)

print(res.text)
```
###### 方法2
```
import requests

url = "http://test/api/auth/me"

params = {"token":"xxxx"}

res = requests.get(url=url,params=params)

print(res.text)
```
