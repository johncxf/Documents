# CURL

curl 是常用的命令行工具，用来请求 Web 服务器

## 基础语法

```shell
$ curl 'https://www.example.com' 
```

### 参数

#### -X

`-X`参数指定 HTTP 请求的方法。

```shell
$ curl -X GET https://www.example.com
$ curl -X POST https://www.example.com
...
```

#### -H

`-H`参数添加 HTTP 请求头

```sh
$ curl -d '{"login": "emma", "pass": "123"}' -H 'Content-Type: application/json' https://xxx.com/login
```

#### -d

`-d`参数用于发送 POST 请求的数据体。使用`-d`参数以后，HTTP 请求会自动加上标头`Content-Type : application/x-www-form-urlencoded`。并且会自动将请求转为 POST 方法，因此可以省略`-X POST`

```shell
$ curl -d'login=admin＆password=123456'-X POST https://xxx.com/login
# 或者
$ curl -d 'login=admin' -d 'password=123456' -X POST  https://xxx.com/login
```

#### -i

`-i`参数打印出服务器回应的 HTTP 标头

```shell
$ curl -i https://www.example.com
```

#### -I

`-I`（`--head`）参数向服务器发出 HEAD 请求，然会将服务器返回的 HTTP 标头打印出来。

```shell
$ curl -I https://www.example.com
```

