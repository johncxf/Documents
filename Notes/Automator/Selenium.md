# Selenium

selenium 是一个自动化操控工具，支持对web端进行自动化操控，从而实现自动化测试。

相关文档：

- https://python-selenium-zh.readthedocs.io/zh-cn/latest/
- https://www.selenium.dev/documentation/

### 安装配置

环境依赖：

- python3.7+

安装：

```sh
$ pip install selenium
```

### 使用

基本使用示例：

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service

# chrome 启动参数
chrome_options = Options()
# 禁用浏览器正在被自动化程序控制的提示
chrome_options.add_experimental_option('excludeSwitches', ['enable-automation', 'enable-logging'])

# 启动
driver = webdriver.Chrome(options=chrome_options)

# 跳转页面
driver.get('https://blog.yiqiesuifeng.cn/')

# 其他操作
ele = driver.find_element_by_xpath('xpath 表达式') # 根据 xpath 获取元素对象
ele.click() # 点击
ele.input() # 输入
...

# 关闭
driver.quit()
```

selenium默认启动本机自带的chrome浏览器，如需指定浏览器路径，则可以这样：

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

# 指定 chromedriver 路径
chromedriver_path = r"chromedriver 路径"
service = Service(executable_path=chromedriver_path)

# chrome 启动参数
chrome_options = Options()

# 指定 chrome 路径
chrome_path = r"chrome 路径"
chrome_options.binary_location = chrome_path

# 禁用浏览器正在被自动化程序控制的提示
chrome_options.add_experimental_option('excludeSwitches', ['enable-automation', 'enable-logging'])

# 启动
driver = webdriver.Chrome(service=service, options=chrome_options)

# 跳转页面
driver.get('https://blog.yiqiesuifeng.cn/')

# 关闭
driver.quit()
```

chromedriver版本需要支持对于chrome的版本

- chromedriver下载地址：https://registry.npmmirror.com/binary.html?path=chromedriver/
- chromium下载地址：https://registry.npmmirror.com/binary.html?path=chromium-browser-snapshots/