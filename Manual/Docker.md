# Docker

Docker 基于 Go 语言开发，是一个基于 LXC 技术之上构建的 Container 容器引擎。容器是一种以固定格式打包软件的方式，以便让软件可以在共享的操作系统中运行，不同于虚拟机，容器并不需要捆绑这个操作系统，只需要软件正常工作所必须的库和设置即可，这使得容器更加高效、轻量级、可以自成系统并且不管部署在什么地方都可以保证运行结果一致。

## 概念

Docker 由镜像（Image）、容器（Container）、仓库（Repository） 三部分组成。

### 镜像

镜像可以简单的类比为电脑装系统用的系统盘，包括操作系统，以及必要的软件。

**Docker 镜像** 是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 **不包含** 任何动态数据，其内容在构建之后也不会被改变。

查看当前所有的镜像

```sh
$ docker images

REPOSITORY            TAG          IMAGE ID       CREATED        SIZE
laradock-php-fpm      latest       3f9514182cdd   20 hours ago   769MB
laradock-workspace    latest       13be4b819932   20 hours ago   1.31GB
laradock-nginx        latest       2092b501d031   21 hours ago   38MB
laradock-mysql        latest       576a63b0cd7e   43 hours ago   592MB
docker                20.10-dind   4f666f62a6ef   3 days ago     336MB
laradock-redis        latest       6f4c54cc20a9   6 days ago     111MB
laradock-phpmyadmin   latest       85f19923c3fd   9 days ago     488MB
```

- REPOSITORY：仓库名称
- TAG： 镜像标签，其中 lastest 表示最新版本
- IMAGE ID：镜像唯一ID
- CREATED：创建时间
- SIZE：镜像大小

### 容器

容器可以简单理解为提供了系统硬件环境，

### 仓库

Docker 的仓库用于存放镜像，类似 Git。可以从中心仓库下载镜像，也可以从自建仓库下载。同时，我们可以把制作好的镜像 commit 到本地，然后 push 到远程仓库。仓库分为公开仓库和私有仓库，最大的公开仓库是官方仓库 Dock Hub。

## 安装配置

### MacOS

#### Homebrew 安装

```sh
$ brew install --cask docker
```

#### 下载安装

下载docker安装包进行安装即可

- https://docs.docker.com/desktop/install/mac-install/

#### 使用

在终端通过命令检查安装后的 Docker 版本在终端通过命令检查安装后的 Docker 版本

```sh
$ docker --version

Docker version 20.10.24, build 297e128
```

## 常用指令

### 镜像

#### 查看当前所有的镜像

```sh
$ docker images
$ docker image ls
$ docker image ls -a
```

#### 删除本地镜像

```sh
$ docker image rm [选项] <镜像1> [<镜像2> ...]
```

其中，`<镜像>` 可以是 `镜像短 ID`、`镜像长 ID`、`镜像名` 或者 `镜像摘要`。

使用镜像短 ID 删除，一般取前3个字符以上，只要足够区分于别的镜像就可以了

如，删除 `laradock-php-fpm` 镜像

```sh
$ docker images

REPOSITORY            TAG          IMAGE ID       CREATED        SIZE
laradock-php-fpm      latest       3f9514182cdd   20 hours ago   769MB
laradock-workspace    latest       13be4b819932   20 hours ago   1.31GB
laradock-nginx        latest       2092b501d031   21 hours ago   38MB
laradock-mysql        latest       576a63b0cd7e   43 hours ago   592MB
docker                20.10-dind   4f666f62a6ef   3 days ago     336MB
laradock-redis        latest       6f4c54cc20a9   6 days ago     111MB
laradock-phpmyadmin   latest       85f19923c3fd   9 days ago     488MB

$ docker rm 3f9
```

使用镜像名删除：

```sh
$ docker image rm laradock-php-fpm
```

更精确的是使用 `镜像摘要` 删除镜像：

```sh
$ docker image ls --digests
$ docker image rm ${digests}
```

### 容器

#### 查看所有容器

```sh
$ docker ps -a
```

#### 启动容器

新建并启动：

```sh
 $ docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

启动已终止容器：

```SH
$ docker container start [OPTIONS] CONTAINER [CONTAINER...]
```

#### 终止容器

```sh
$ docker container stop [OPTIONS] CONTAINER [CONTAINER...]
```

#### 进入容器

##### attach

```sh
$ docker attach 243c
```

##### exec

```sh
$ docker exec -it 69d1 bash
```

### 其他

```sh
# 查看docker的版本
$ docker -v

# 查看镜像历史
$ docker history

# 查看docker的详细信息
$ docker info

# 构建自己的镜像xx/ownname是自己的托管代码路径
$ docker build -t xx/ownname <Dockerfile路径>

# 进入容器内部查看或操作
$ docker exec it containername /bin/bash

# 查看某容器的详细配置信息
$ docker inspect container-name

# 加载镜像
$ docker load < /home/save.tar

# 登录某镜像平台，默认是hub.docker.com，需要注册账户
$ docker login <daocloud.io>

# 查看容器日志
$ docker logs -f <容器名或ID>

# 暂停某一容器所有进程
$ docker pause <docker name or id>

# 从远程拉取镜像，不带tag视为latest
$ docker pull <镜像名:tag>

# 将镜像推送到远程仓库
$ docker push name:tag

# 删除所有容器
$ docker rm ${docker ps -a -q}

# 删除某容器
$ docker rm -f <容器名或ID>

# 强制删除某镜像
$ docker rmi -f <镜像名或ID>

# 运行一个容器,命名、设置端口映射、后台运行等
$ docker run -it -d -p --name

# 保存镜像
$ docker save busy-box>/home/save.tar

# 标记本地镜像
$ docker tag

# 回复某一容器的所有进程
$ docker unpause <docker name or id>
```

官方文档：https://docs.docker.com/engine/reference/commandline/docker/

## Docker Compose

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

Compose 使用的三个步骤：

- 使用 Dockerfile 定义应用程序的环境。
- 使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
- 最后，执行 docker-compose up 命令来启动并运行整个应用程序。

### 常用指令

`docker-compose` == `docker compose`

`docker-compose` 命令的基本的使用格式是

```sh
$ docker-compose [-f=<arg>...] [options] [COMMAND] [ARGS...]
```

#### Options：

- `-h, --help`：帮助
- `-v, --version`：版本
- `-f, --file FILE`：指定使用的 Compose 模板文件，默认为 `docker-compose.yml`，可以多次指定
- `-p, --project-name NAME`：指定项目名称，默认将使用所在目录名称作为项目名
- `--verbose`：输出更多调试信息
- ...

#### Commands:

##### version

打印版本信息打印版本信息

```sh
$ docker-compose version
```

##### help

获得一个命令的帮助

```sh
$ docker-compose help
```

##### images

列出 Compose 文件中包含的镜像

```sh
$ docker-compose images
```

##### top

查看各个服务容器内运行的进程

```sh
$ docker-compose top
```

##### ps

列出项目中目前的所有容器

```sh
$ docker-compose ps [options] [SERVICE...]
```

options：

- `-q`：只打印容器的 ID 信息。

##### start

启动已经存在的服务容器

```sh
$ docker-compose start [SERVICE...]
```

##### stop

停止已经处于运行状态的容器，但不删除它。通过 `docker-compose start` 可以再次启动这些容器。

```sh
$ docker-compose stop [options] [SERVICE...]
```

options：

- `-t, --timeout TIMEOUT`：停止容器时候的超时（默认为 10 秒）。

##### restart

重启项目中的服务

```sh
$ docker-compose restart [options] [SERVICE...]
```

options：

- `-t, --timeout TIMEOUT`：指定重启前停止容器的超时（默认为 10 秒）。

##### up

自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作

```sh
$ docker-compose up [options] [SERVICE...]
```

options：

- `-d`：在后台运行服务容器
- `--no-color`：不使用颜色来区分不同的服务的控制台输出
- `--no-deps`：不启动服务所链接的容器。
- `--force-recreate`：强制重新创建容器，不能与 `--no-recreate` 同时使用
- `--no-recreate`：如果容器已经存在了，则不重新创建，不能与 `--force-recreate` 同时使用
- `--no-build`：不自动构建缺失的服务镜像
- `-t, --timeout TIMEOUT`：停止容器时候的超时（默认为 10 秒）

##### down

此命令将会停止 `up` 命令所启动的容器，并移除网络

```sh
$ docker-compose down
```

##### port

打印某个容器端口所映射的公共端口

```sh
$ docker-compose port [options] SERVICE PRIVATE_PORT
```

options：

`--protocol=proto`：指定端口协议，tcp（默认值）或者 udp

`--index=index`：如果同一服务存在多个容器，指定命令对象容器的序号（默认为 1）

##### build

构建（重新构建）项目中的服务容器。

服务容器一旦构建后，将会带上一个标记名，例如对于 web 项目中的一个 db 容器，可能是 web_db。

```sh
$ docker-compose build [options] [SERVICE...]
```

options：

- `--force-rm`：删除构建过程中的临时容器
- `--no-cache`：构建镜像过程中不使用 cache（这将加长构建过程）
- `--pull`：始终尝试通过 pull 来获取更新版本的镜像

##### config

验证 Compose 文件格式是否正确，若正确则显示配置，若格式错误显示错误原因

```sh
$ docker-compose config
```

##### exec

进入指定的容器

```sh
$ docker-compose exec
```

##### kill

通过发送进程信号来强制停止服务容器

```sh
$ docker-compose kill [options] [SERVICE...]
```

options：

-  `-s` 参数来指定发送的信号，例如通过如下指令发送 `SIGINT` 信号。

##### logs

查看服务容器的输出。默认情况下，docker-compose 将对不同的服务输出使用不同的颜色来区分。可以通过 `--no-color` 来关闭颜色

```sh
$ docker-compose logs [options] [SERVICE...]
```

##### pull

拉取服务依赖的镜像

```sh
$ docker-compose pull [options] [SERVICE...]
```

选项：

- `--ignore-pull-failures`：忽略拉取镜像过程中的错误

##### push

推送服务依赖的镜像到 Docker 镜像仓库

```sh
$ docker-compose push [SERVICE...]
```

##### pause

暂停一个服务容器

```sh
$ docker-compose pause [SERVICE...]
```

##### unpause

恢复处于暂停状态中的服务

```sh
$ docker-compose unpause [SERVICE...]
```

##### rm

删除所有（停止状态的）服务容器。推荐先执行 `docker-compose stop` 命令来停止容器。

```sh
$ docker-compose rm [options] [SERVICE...]
```

options：

- `-f, --force` 强制直接删除，包括非停止状态的容器。一般尽量不要使用该选项
- `-v` 删除容器所挂载的数据卷

##### run

在指定服务上执行一个命令

```sh
$ docker-compose run [options] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...]
```

例如：

```sh
$ docker-compose run ubuntu ping docker.com
```

将会启动一个 ubuntu 服务容器，并执行 `ping docker.com` 命令。

默认情况下，如果存在关联，则所有关联的服务将会自动被启动，除非这些服务已经在运行中。

该命令类似启动容器后运行指定的命令，相关卷、链接等等都将会按照配置自动创建。

两个不同点：

- 给定命令将会覆盖原有的自动运行命令；
- 不会自动创建端口，以避免冲突。

如果不希望自动启动关联的容器，可以使用 `--no-deps` 选项，例如

```sh
$ docker-compose run --no-deps web python manage.py shell
```

将不会启动 web 容器所关联的其它容器。

options：

- `-d` 后台运行容器。
- `--name NAME` 为容器指定一个名字。
- `--entrypoint CMD` 覆盖默认的容器启动指令。
- `-e KEY=VAL` 设置环境变量值，可多次使用选项来设置多个环境变量。
- `-u, --user=""` 指定运行容器的用户名或者 uid。
- `--no-deps` 不自动启动关联的服务容器。
- `--rm` 运行命令后自动删除容器，`d` 模式下将忽略。
- `-p, --publish=[]` 映射容器端口到本地主机。
- `--service-ports` 配置服务端口并映射到本地主机。
- `-T` 不分配伪 tty，意味着依赖 tty 的指令将无法运行。

##### scale

设置指定服务运行的容器个数

```sh
$ docker-compose scale [options] [SERVICE=NUM...]
```

通过 `service=num` 的参数来设置数量。例如：

```sh
$ docker-compose scale web=3 db=2
```

将启动 3 个容器运行 web 服务，2 个容器运行 db 服务。

一般的，当指定数目多于该服务当前实际运行容器，将新创建并启动容器；反之，将停止容器。

options：

- `-t, --timeout TIMEOUT` 停止容器时候的超时（默认为 10 秒）。

## Laradock

Laradock 是 Docker 的一个完整的PHP开发环境。

- 官网：https://laradock.io/


### 安装配置

##### clone库

在自定义目录下拉取 laradock 代码

```sh
$ git clone https://github.com/Laradock/laradock.git
```

laradock 和项目目录关系可以同级：

```
* laradock
* project-1
* project-2
```

##### 配置文件

这里以配置多个web项目为例进行配置

进入 `laradock` 目录将 `env-example` 重命名为 `.env`：

```sh
$ cp .env.example .env
```

这个是用于指定你的应用程序的目录，默认是在 Laradock 的上一层:

```text
APP_CODE_PATH_HOST=../
```

**记得最后要以 `/` 结尾。**

##### 构建容器

根据需要构建，如果没有用到 redis 和 phpmyadmin，可以从命令中去掉

```sh
$ docker-compose up -d nginx mysql phpmyadmin redis workspace
```

#### PHP

**修改PHP版本**

修改.env配置文件版本

```text
PHP_VERSION=7.3
```

最后重建镜像

```sh
$ docker-compose build php-fpm
```

**修改PHP-CLI版本**

修改.env配置文件`PHP_VERSION`

```text
PHP_VERSION=7.3
```

最后重建图像，执行php-cli是在workspace镜像的容器中所以需要重新构建workspace

```sh
$ docker-compose build workspace
```

#### Nginx

##### 站点配置

- 进入nginx配置文件目录：`cd nginx/sites`

- 生成该站点的配置文件：`cp laravel.conf.example laradmin.conf`

- 配置该文件，相关配置参考nginx的配置：

  ```nginx
  server_name laradmin.test;
  # 开头必须是/var/www/，映射.env文件中APPLICATION配置对应的目录
  root /var/www/laradmin/public;
  ```

- 执行以下命令：

  ```sh
  $ docker-compose build nginx
  $ docker-compose restart nginx
  ```

- 修改 `/etc/hosts` 文件内容：`sudo vim /etc/hosts`

  ```
  127.0.0.1 laradmin.test
  ```

#### Mysql

默认情况下使用MySQL 最新版本运行，用户和密码默认都是 root

##### 修改版本

- 修改.env laradock配置文件 MYSQL_VERSION=5.7，具体可用版本见：https://store.docker.com/images/mysql
- 重新编译 `docker-compose build mysql`
- 如果已经运行则重新启动 `docker-compose restart mysql`

MacOS M1 buiild 时候如果出现报错：

```
[+] Building 0.2s (3/3) FINISHED
 => [internal] load build definition from Dockerfile                                                         
 => => transferring dockerfile: 32B                                                                           
 => [internal] load .dockerignore                                                                             
 => => transferring context: 2B                                                                               
 => ERROR [internal] load metadata for docker.io/library/mysql:5.7                                           
------
 > [internal] load metadata for docker.io/library/mysql:5.7:
------
failed to solve: rpc error: code = Unknown desc = failed to solve with frontend dockerfile.v0: failed to create LLB definition: no match for platform in manifest sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94: not found
```

可以修改 `docker-compose.yml`新增 `platform: linux/amd64` 配置

```
### MySQL ################################################
    mysql:
      build:
        context: ./mysql
        args:
          - MYSQL_VERSION=${MYSQL_VERSION}
      environment:
        - MYSQL_DATABASE=${MYSQL_DATABASE}
        - MYSQL_USER=${MYSQL_USER}
        - MYSQL_PASSWORD=${MYSQL_PASSWORD}
        - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
        - TZ=${WORKSPACE_TIMEZONE}
      volumes:
        - ${DATA_PATH_HOST}/mysql:/var/lib/mysql
        - ${MYSQL_ENTRYPOINT_INITDB}:/docker-entrypoint-initdb.d
      ports:
        - "${MYSQL_PORT}:3306"
      networks:
        - backend
      platform: linux/amd64
```

##### 其他用法

```sh
# 进入 Mysql 容器
$ docker-compose exec mysql bash

# root 登陆
$ mysql -uroot -proot

# 其他用户
$ mysql -udefault -psecret

# 查看日志
$ docker-compose logs mysql
```

mysql 数据目录：`~/.laradock/data/mysql`

#### phpMyAdmin

`phpmyadmin`使用该`docker-compose up`命令运行phpMyAdmin

```sh
$ docker-compose up -d mysql phpmyadmin
```

打开浏览器并访问端口**8080**上的localhost ： `http://localhost:8080`，登录信息如下

```text
host: mysql
user: root
password: root
```

