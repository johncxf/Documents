# Python

Python是著名的“龟叔”Guido van Rossum在1989年圣诞节期间，为了打发无聊的圣诞节而编写的一个编程语言。

### 基础语法

#### 行和缩进

Python的代码块不使用大括号（{}）来控制类，函数以及其他逻辑判断。python最具特色的就是用缩进来写模块

#### 注释

单行注释`#`开头

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
# 文件名：test.py

# 第一个注释
print "Hello, Python!"; # 第二个注释
```

多行注释使用三个单引号(''')或三个双引号(""")

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
# 文件名：test.py

'''
这是多行注释，使用单引号。
这是多行注释，使用单引号。
这是多行注释，使用单引号。
'''

"""
这是多行注释，使用双引号。
这是多行注释，使用双引号。
这是多行注释，使用双引号。
"""
```

#### 空行

函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。

**注：**空行也是程序代码的一部分。

### 类库

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

```python
from selenium import webdriver

browser = webdriver.Chrome()
try:
	browser.get('https://www.baidu.com')
finally:
	browser.close()
```

**声明浏览器对象**

```python
from seleinum import webdriver

browser = webdriver.Chrome()
browser = webdriver.Firefox()
browser = webdriver.Edge()
browser = webdriver.Safari()
browser = webdriver.Phantom.JS()
```







