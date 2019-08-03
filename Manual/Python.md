### Python学习手册

#### urllib

> HTTP库

- urllib.request             请求模块
- urllib.error                 异常处理模块
- urllib.parse                url解析模块
- urllib.robotparser    robots.txt解析模块

**Python2**：

```python
import urllib2
response = urllib2.urlopen('https://www.baidu.com')
```

**Python3:**

```python
import urllib.request
response = urllib.request.urlopen('https://www.baidu.com')
```

#### Requests

> Python语言白编写，基于urllib，采用Apache2 Licensed 开源协议的HTTP库

```python
import requests

response = requests.get('https://www.baidu.com')
print(type(response))
print(response.status_code)
print(response.text)
print(response.cookies)
```

**请求方式**

```python
import requests

requests.post('https://www.baidu.com')
requests.put('https://www.baidu.com')
requests.delete('https://www.baidu.com')
requests.head('https://www.baidu.com')
requests.options('https://www.baidu.com')
```

**响应**

```python
import requests

 response = requests.grt('https://www.baidu.com')
 print(type(response.status_code),response.status_code);
 print(response.headers);
 print(response.url);
 print(response.history);
```

#### Selenium

**基本使用**

```
from selenium import webdriver

browser = webdriver.Chrome()
try:
	browser.get('https://www.baidu.com')
finally:
	browser.close()
```

**声明浏览器对象**

```
from seleinum import webdriver

browser = webdriver.Chrome()
browser = webdriver.Firefox()
browser = webdriver.Edge()
browser = webdriver.Safari()
browser = webdriver.Phantom.JS()

```











