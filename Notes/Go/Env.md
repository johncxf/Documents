# 部署

## 独立部署

对于简单的项目，通常我们只需要将编译后的二进制文件拷贝到服务器上，然后设置为后台守护进程运行即可。

本文以项目：https://github.com/johncxf/go_practice 为例

#### 编译

编译为 linux 系统可执行的二进制文件，二进制文件为 `./bin/go-api`，可自行修改

```sh
$ CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o ./bin/go-api
```

可以使用 `-ldflags "-s -w"`参数去掉符号表和调试信息，可以压缩二进制文件大小

```sh
$ CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags "-s -w" -o ./bin/go-api
```

大小对比：

```
$ ll ./bin
total 92168
-rwxr-xr-x  1 chen  staff    27M  1 16 22:03 go-api
-rwxr-xr-x  1 chen  staff    18M  1 16 22:53 go-api-1
```

#### 上传文件

在服务器新建一个目录，用来存放该项目：

```sh
$ mkdir /www/wwwroot/go-project & cd /www/wwwroot/go-project
```

上传编辑的二进制文件到对应项目目录下，并将静态资源文件、配置文件等项目依赖的文件同时上传

```sh
.
├─ bin
| ├─ go-api
├─ config
| └─ env.yml
```

#### 启动服务

##### 直接启动：

```sh
# 在项目根目录下
$ ./bin/go-api config/env.yml
```

直接启动可以键盘 control + c 直接退出即可

##### 后台启动：

这里使用`nohup`进行启动，也可以使用其他进程管理工具进行启动（如：supervisor）

```sh
$ sudo nohup ./bin/go-api config/env.yml > nohup_go-api.log 2>&1 &

# 执行后控制台会输出进程号
[1] 19935
```

- `./bin/go-api config/env.yml`：是我们应用程序的启动命令
- `nohup ... &`表示在后台不挂断的执行上述应用程序的启动命令
- `> nohup_api-admin.log`表示将命令的标准输出重定向到 nohup_api-adminlog 文件
- `2>&1`表示将标准错误输出也重定向到标准输出中，结合上一条就是把执行命令的输出都定向到 nohup_api-admin.log 文件

后台启动找到对应进程，直接 kill 即可

```sh
$ ps -ef | grep go-api

[root@VM-0-11-centos go-project]# ps -ef | grep go-api
root     19935 32326  0 22:47 pts/0    00:00:00 sudo nohup ./bin/go-api config/env.yml
root     19936 19935  0 22:47 pts/0    00:00:00 ./bin/go-api config/env.yml
root     28488 32326  0 23:12 pts/0    00:00:00 grep --color=auto go-api

$ kill -9 19936
```

其他指令：

```sh
# 查看所有后台进程
$ jobs -l
```

#### 端口开放

腾讯云或者阿里云等服务器都需要开放对于的端口，才能进行访问。

以腾讯云为例，在控制台设置对于服务器的安全组规则，在安全组中添加一条对应端口开放的规则即可，实际操作可以参考对应服务器服务商提供的文档。

#### 访问

访问：`http://服务器公网ip:端口/uri`即可使用

如果需要通过域名访问，则需要搭配 nginx 来使用。

### 结合Nginx部署

在需要静态文件分离、需要配置多个域名及证书、需要自建负载均衡层等稍复杂的场景下，我们一般需要搭配第三方的web服务器（Nginx、Apache）来部署我们的程序。

首先需要安装 nginx，nginx 安装教程这里不再赘述。

这里简单介绍通过 nginx 做反向代理，实现通过域名访问Go应用的示例。

修改 nginx 配置，配置反向代理即可：

```nginx
...
http {
    ...
    server {
        # 端口
        listen       80;
        # 域名
        server_name  api.yiqiesuifeng.cn;

        access_log   /var/log/api-access.log;
        error_log    /var/log/api-error.log;

		# 静态文件请求
        location ~ .*\.(gif|jpg|jpeg|png|js|css|eot|ttf|woff|svg|otf)$ {
            access_log off;
            expires    1d;
            root       /www/wwwroot/go-project;
        }

        # index.html页面请求
        location / {
            root /www/wwwroot/go-project;
            index index.html;
            try_files $uri $uri/ /index.html;
        }
		
        # API 请求
        location /api {
            # 代理到go服务
            proxy_pass                 http://127.0.0.1:8088;
            proxy_redirect             off;
            proxy_set_header           Host             $host;
            proxy_set_header           X-Real-IP        $remote_addr;
            proxy_set_header           X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
    }
}

```

修改 ngxin 配置后重新 nginx 即可。

## Docker 部署

不管是开发还是生产环境，通过 docker 方式部署服务都是一种不错的选择，能够解决不同开发环境一致性的问题。

本文以项目：https://github.com/johncxf/go_practice 为例。

#### Dockerfile 构建 Go 运用环境

在项目根目录下添加 Dockerfile 文件：

```dockerfile
FROM golang:alpine

# 在容器内部设置环境变量
ENV GO111MODULE=on \
    GOPROXY=https://goproxy.cn,direct \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# 设置后续指令的工作目录
WORKDIR /build

# 复制项目中的 go.mod 和 go.sum文件并下载依赖信息
COPY go.mod .
COPY go.sum .
RUN go mod download

# 将代码复制到容器中
COPY . .

# 将代码编译成二进制可执行文件
RUN go build -o go-api .

WORKDIR /dist

RUN cp /build/go-api .

COPY ./config ./config

# 需要运行的命令
ENTRYPOINT ["/dist/go-api", "/dist/config/env.yml"]
```

如果需要缩小镜像大小，则可以用以下方式进行构建

```dockerfile
FROM golang:alpine AS builder

# 在容器内部设置环境变量
ENV GO111MODULE=on \
    GOPROXY=https://goproxy.cn,direct \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# 设置后续指令的工作目录
WORKDIR /build

# 复制项目中的 go.mod 和 go.sum文件并下载依赖信息
COPY go.mod .
COPY go.sum .
RUN go mod download

# 将代码复制到容器中
COPY . .

# 将代码编译成二进制可执行文件
RUN go build -o go-api .


# 创建一个小镜像
#FROM scratch
FROM debian:stretch-slim

COPY ./config /config

# 从builder镜像中把 /build/go-api 拷贝到当前目录
COPY --from=builder /build/go-api /

# 需要运行的命令
ENTRYPOINT ["/go-api", "config/env.yml"]
```

注：镜像`scratch`没有bash，会导致无法通过`docker exec`进入容器，建议直接改成`debian:stretch-slim`镜像

构建 docker 镜像 `go-web-app`：

```sh
$ docker build -t go-web-app .

# 查看镜像
$ docker images
```

启动容器来运行镜像：

```sh
$ docker run -p 8888:8088 go-web-app

# 后台启动
$ $ docker run -p 8888:8088 -d go-web-app

# 查看容器
$ docker ps -a
CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS          PORTS                    NAMES
959ee47f203b   go-web-app   "/go-api config/env.…"   23 seconds ago   Up 22 seconds   0.0.0.0:8888->8088/tcp   nostalgic_mclean
```

标志位`-p`用来定义端口绑定，容器中go服务端口为 8088，绑定到本机端口 8888，接下来就可以通过`http://127.0.0.1:8888`进行访问了。

#### Mysql 容器

##### 拉取 Mysql 镜像：

```sh
# 拉取最新版本 mysql，也可以指定版本
$ docker pull mysql:latest

# 查看镜像
$ docker images
```

##### 运行 Mysql 容器：

```sh
$ docker run --name mysql -p 8306:3306 -e MYSQL_ROOT_PASSWORD=root123456 -v ~/docker-data/mysql:/var/lib/mysql -d mysql:latest

# 查看容器
$ docker ps -a
```

- `--name mysql-latest`：容器命名为 mysql-latest
- `-p 8306:3306`：映射容器服务的 `3306` 端口到宿主机的 `8306` 端口，外部主机可以直接通过 宿主机`ip:8306` 访问到MySQL服务
- `-e MYSQL_ROOT_PASSWORD=root123456`：设置 MySQL 服务 root 用户密码
- `-v /Users/john/docker-data/mysql:/var/lib/mysql`：表示将宿主机的`/Users/john/docker-data/mysql`卷映射到容器里的 `/var/lib/mysql`卷中，为了把数据保存在宿主机中，防止容器删掉就没了
- `-d`：后台运行
- `mysql:latest`：使用这个mysql镜像

##### 连通性测试：

进入 docker 容器连接 mysql：

```sh
# 进入容器
$ docker exec -it mysql-latest /bin/sh
# 连接 mysql
$ mysql -uroot -proot123456 -h 127.0.0.1 -P3306
```

或者本机可以通过 navicat 等工具进行连接。

#### Redis 容器

##### 拉取 Redis 镜像：

```sh
$ docker pull redis

# 查看镜像
$ docker images
```

##### 运行 Redis 容器：

```sh
$ docker run --name redis -v ~/docker-data/redis:/usr/local/redis -p 6379:6379 -d redis:latest --requirepass "root123456"

# 查看容器
$ docker ps -a
```

其他参数同mysql配置，`--requirepass` 用来设置 Redis 密码

##### 连通性测试：

直接在本地使用 `redis-cli` 测试 redis 连通性：

```sh
> redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> auth root123456
OK
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```

进入容器：

```sh
> docker exec -it redis /bin/sh
# redis-cli -p 6379
127.0.0.1:6379> auth root123456
OK
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> exit
# exit
```

#### 修改配置

由于要连接不同容器内的数据库，需要修改项目数据库配置HOST地址为容器名，这里以我的项目配置 `env.yml`为例子：

```yaml
# MySQL 配置
mysql:
#  host: 127.0.0.1 # 服务器地址
  host: mysql
  port: 3306 # 端口
  db_name: test # 数据库名称
  username: root # 数据库用户名
  password: root123456 # 数据库密码
  ...
  
# Redis 配置
redis:
#  host: 127.0.0.1 # 地址
  host: redis
  port: 6379 # 端口
  db: 0
  password: root123456 # 密码
```

然后重新 build Go服务镜像：

```sh
$ docker build -t go-web-app .
```

重新启动容器，并使用`--link`关联Mysql、Redis容器：

```sh
$ docker run --name go-api --link=mysql:mysql --link=redis:redis -p 8888:8088 -d go-web-app
```

注：使用这种方式构建环境，每次都需要单独启动Mysql、Redis、go服务容器，并通过 `--link`关联容器

## Docker Compose 部署

在使用docker部署时，除了使用`--link`的方式来关联容器之外，还可以使用 docker compose 运行多个容器。

#### 定义 Dockerfile

我这里用于区分默认 Dockerfile 文件，在项目根目录下新建一个 Dockerfile-compose 文件：

```dockerfile
FROM golang:alpine AS builder

# 在容器内部设置环境变量
ENV GO111MODULE=on \
    GOPROXY=https://goproxy.cn,direct \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# 设置后续指令的工作目录
WORKDIR /build

# 复制项目中的 go.mod 和 go.sum文件并下载依赖信息
COPY go.mod .
COPY go.sum .
RUN go mod download

# 将代码复制到容器中
COPY . .

# 将代码编译成二进制可执行文件
RUN go build -o go-api .


# 创建一个小镜像
#FROM scratch
FROM debian:stretch-slim

COPY ./config /config

# 从builder镜像中把 /build/go-api 拷贝到当前目录
COPY --from=builder /build/go-api /

# 需要运行的命令（docker compose 运行不需要执行这一行）
#ENTRYPOINT ["/go-api", "config/env.yml"]
```

#### docker-compose.yml

新建 `docker-compose.yml`配置文件与项目根目录下。

我这里配置了mysql、redis、go-api三个容器，配置以及说明如下：

```yaml
version: "3.7"
services:
  mysql:
    # 镜像版本号
    image: mysql:8.0.33
    # 容器名
    container_name: go-web-mysql
    # 端口号映射
    ports:
      - "8306:3306"
    # 失败后总是重启
    restart: "always"
    command: "--default-authentication-plugin=mysql_native_password --init-file /data/application/init.sql"
    environment:
      MYSQL_ROOT_PASSWORD: "root123456" # root 账号密码
      MYSQL_DATABASE: "test"            # 数据库
    # 将mysql相关数据挂载到本机目录
    volumes:
      - ~/docker-data/go-api/mysql/init.sql:/data/application/init.sql
      - ~/docker-data/go-api/mysql/data:/var/lib/mysql           #数据文件挂载
      - ~/docker-data/go-api/mysql/conf.d:/etc/mysql/conf.d      #配置文件挂载
      - ~/docker-data/go-api/mysql/log:/var/log/mysql            #日志文件挂载
  redis:
    # 镜像版本号
    image: redis:7.2.4
    # 容器名
    container_name: go-web-redis
    # 端口号
    ports:
      - "6379:6379"
    # 失败后总是重启
    restart: "always"
    # 以配置文件的方式启动 redis.conf
    command: "redis-server /etc/redis/redis.conf --appendonly yes --requirepass root123456"
    # 文件夹以及文件映射
    volumes:
      - ~/docker-data/go-api/redis:/data
      - ~/docker-data/go-api/redis/redis.conf:/etc/redis/redis.conf
  go-api:
    # 容器名
    container_name: go-web-api
    build:
      context: .
      dockerfile: Dockerfile-compose  # 默认为 Dockerfile，这里重新定义为 Dockerfile-compose 文件
    # 失败后总是重启
    restart: "always"
    #    command: sh -c "./wait-for-it.sh mysql:3306 -- ./go-api ./config/env.yml"
    command: [ "/wait-for-it.sh", "mysql:3306", "--", "/go-api", "config/env.yml" ]
    # 依赖启动项
    depends_on:
      - mysql
      - redis
    # 端口映射
    ports:
      - "8888:8088"
```

#### Mysql 状态检测

docker-compose.yml 配置文件中 `depends_on`字段仅能保证web服务启动时，mysql服务处于Running状态而不是Ready状态，因为go-api需要等待mysql启动后再启动，因此需要添加一个`wait-for-it.sh`脚本文件，检测mysql服务是否处于Ready状态。

在项目根目录下新建 `wait-for-it.sh` 文件

```sh
#!/usr/bin/env bash
# Use this script to test if a given TCP host/port are available

WAITFORIT_cmdname=${0##*/}

echoerr() { if [[ $WAITFORIT_QUIET -ne 1 ]]; then echo "$@" 1>&2; fi }

usage()
{
    cat << USAGE >&2
Usage:
    $WAITFORIT_cmdname host:port [-s] [-t timeout] [-- command args]
    -h HOST | --host=HOST       Host or IP under test
    -p PORT | --port=PORT       TCP port under test
                                Alternatively, you specify the host and port as host:port
    -s | --strict               Only execute subcommand if the test succeeds
    -q | --quiet                Don't output any status messages
    -t TIMEOUT | --timeout=TIMEOUT
                                Timeout in seconds, zero for no timeout
    -- COMMAND ARGS             Execute command with args after the test finishes
USAGE
    exit 1
}

wait_for()
{
    if [[ $WAITFORIT_TIMEOUT -gt 0 ]]; then
        echoerr "$WAITFORIT_cmdname: waiting $WAITFORIT_TIMEOUT seconds for $WAITFORIT_HOST:$WAITFORIT_PORT"
    else
        echoerr "$WAITFORIT_cmdname: waiting for $WAITFORIT_HOST:$WAITFORIT_PORT without a timeout"
    fi
    WAITFORIT_start_ts=$(date +%s)
    while :
    do
        if [[ $WAITFORIT_ISBUSY -eq 1 ]]; then
            nc -z $WAITFORIT_HOST $WAITFORIT_PORT
            WAITFORIT_result=$?
        else
            (echo -n > /dev/tcp/$WAITFORIT_HOST/$WAITFORIT_PORT) >/dev/null 2>&1
            WAITFORIT_result=$?
        fi
        if [[ $WAITFORIT_result -eq 0 ]]; then
            WAITFORIT_end_ts=$(date +%s)
            echoerr "$WAITFORIT_cmdname: $WAITFORIT_HOST:$WAITFORIT_PORT is available after $((WAITFORIT_end_ts - WAITFORIT_start_ts)) seconds"
            break
        fi
        sleep 1
    done
    return $WAITFORIT_result
}

wait_for_wrapper()
{
    # In order to support SIGINT during timeout: http://unix.stackexchange.com/a/57692
    if [[ $WAITFORIT_QUIET -eq 1 ]]; then
        timeout $WAITFORIT_BUSYTIMEFLAG $WAITFORIT_TIMEOUT $0 --quiet --child --host=$WAITFORIT_HOST --port=$WAITFORIT_PORT --timeout=$WAITFORIT_TIMEOUT &
    else
        timeout $WAITFORIT_BUSYTIMEFLAG $WAITFORIT_TIMEOUT $0 --child --host=$WAITFORIT_HOST --port=$WAITFORIT_PORT --timeout=$WAITFORIT_TIMEOUT &
    fi
    WAITFORIT_PID=$!
    trap "kill -INT -$WAITFORIT_PID" INT
    wait $WAITFORIT_PID
    WAITFORIT_RESULT=$?
    if [[ $WAITFORIT_RESULT -ne 0 ]]; then
        echoerr "$WAITFORIT_cmdname: timeout occurred after waiting $WAITFORIT_TIMEOUT seconds for $WAITFORIT_HOST:$WAITFORIT_PORT"
    fi
    return $WAITFORIT_RESULT
}

# process arguments
while [[ $# -gt 0 ]]
do
    case "$1" in
        *:* )
        WAITFORIT_hostport=(${1//:/ })
        WAITFORIT_HOST=${WAITFORIT_hostport[0]}
        WAITFORIT_PORT=${WAITFORIT_hostport[1]}
        shift 1
        ;;
        --child)
        WAITFORIT_CHILD=1
        shift 1
        ;;
        -q | --quiet)
        WAITFORIT_QUIET=1
        shift 1
        ;;
        -s | --strict)
        WAITFORIT_STRICT=1
        shift 1
        ;;
        -h)
        WAITFORIT_HOST="$2"
        if [[ $WAITFORIT_HOST == "" ]]; then break; fi
        shift 2
        ;;
        --host=*)
        WAITFORIT_HOST="${1#*=}"
        shift 1
        ;;
        -p)
        WAITFORIT_PORT="$2"
        if [[ $WAITFORIT_PORT == "" ]]; then break; fi
        shift 2
        ;;
        --port=*)
        WAITFORIT_PORT="${1#*=}"
        shift 1
        ;;
        -t)
        WAITFORIT_TIMEOUT="$2"
        if [[ $WAITFORIT_TIMEOUT == "" ]]; then break; fi
        shift 2
        ;;
        --timeout=*)
        WAITFORIT_TIMEOUT="${1#*=}"
        shift 1
        ;;
        --)
        shift
        WAITFORIT_CLI=("$@")
        break
        ;;
        --help)
        usage
        ;;
        *)
        echoerr "Unknown argument: $1"
        usage
        ;;
    esac
done

if [[ "$WAITFORIT_HOST" == "" || "$WAITFORIT_PORT" == "" ]]; then
    echoerr "Error: you need to provide a host and port to test."
    usage
fi

WAITFORIT_TIMEOUT=${WAITFORIT_TIMEOUT:-15}
WAITFORIT_STRICT=${WAITFORIT_STRICT:-0}
WAITFORIT_CHILD=${WAITFORIT_CHILD:-0}
WAITFORIT_QUIET=${WAITFORIT_QUIET:-0}

# Check to see if timeout is from busybox?
WAITFORIT_TIMEOUT_PATH=$(type -p timeout)
WAITFORIT_TIMEOUT_PATH=$(realpath $WAITFORIT_TIMEOUT_PATH 2>/dev/null || readlink -f $WAITFORIT_TIMEOUT_PATH)

WAITFORIT_BUSYTIMEFLAG=""
if [[ $WAITFORIT_TIMEOUT_PATH =~ "busybox" ]]; then
    WAITFORIT_ISBUSY=1
    # Check if busybox timeout uses -t flag
    # (recent Alpine versions don't support -t anymore)
    if timeout &>/dev/stdout | grep -q -e '-t '; then
        WAITFORIT_BUSYTIMEFLAG="-t"
    fi
else
    WAITFORIT_ISBUSY=0
fi

if [[ $WAITFORIT_CHILD -gt 0 ]]; then
    wait_for
    WAITFORIT_RESULT=$?
    exit $WAITFORIT_RESULT
else
    if [[ $WAITFORIT_TIMEOUT -gt 0 ]]; then
        wait_for_wrapper
        WAITFORIT_RESULT=$?
    else
        wait_for
        WAITFORIT_RESULT=$?
    fi
fi

if [[ $WAITFORIT_CLI != "" ]]; then
    if [[ $WAITFORIT_RESULT -ne 0 && $WAITFORIT_STRICT -eq 1 ]]; then
        echoerr "$WAITFORIT_cmdname: strict mode, refusing to execute subprocess"
        exit $WAITFORIT_RESULT
    fi
    exec "${WAITFORIT_CLI[@]}"
else
    exit $WAITFORIT_RESULT
fi
```

#### 构建启动

```sh
# 会根据 docker-compose 文件，构建镜像，并启动所有容器
$ docker-compose up -d

# 查看容器
$ docker-compose ps -a

# 停止所有容器
$ docker-compose down
```

启动成功后，接下来就可以通过`http://127.0.0.1:8888`进行访问了。

如果连接 Mysql 出现报错，可以参考：

Msql 进入容器使用 `127.0.0.1`连接mysql报错：`ERROR 1130 (HY000): Host '127.0.0.1' is not allowed to connect to this MySQL server`，可以直接输入 `mysql` 进入，执行以下操作：

```sh
> use mysql;
> update user set host = '%' where user = 'root';
> FLUSH PRIVILEGES;
```

Mysql root 密码设置不生效解决：

```sh
> use mysql;
> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root123456';
> FLUSH PRIVILEGES;
```

