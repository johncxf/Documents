## Docker

#### 常用指令

| 指令                                        | 说明                                               |
| :------------------------------------------ | :------------------------------------------------- |
| docker -v                                   | 查看docker的版本                                   |
| docker history                              | 查看镜像历史                                       |
| docker info                                 | 查看docker的详细信息                               |
| docker images                               | 查看当前所有的镜像                                 |
| docker ps -a                                | 查看当前启动的、停止的所有容器                     |
| docker build -t xx/ownname <Dockerfile路径> | 构建自己的镜像xx/ownname是自己的托管代码路径       |
| docker exec it containername /bin/bash      | 进入容器内部查看或操作                             |
| docker inspect container-name               | 查看某容器的详细配置信息                           |
| docker load < /home/save.tar                | 加载镜像                                           |
| docker login <daocloud.io>                  | 登录某镜像平台，默认是hub.docker.com，需要注册账户 |
| docker logs -f <容器名或ID>                 | 查看容器日志                                       |
| docker pause <docker name or id>            | 暂停某一容器所有进程                               |
| docker pull <镜像名:tag                     | 从远程拉取镜像，不带tag视为latest                  |
| docker push name:tag                        | 将镜像推送到远程仓库                               |
| docker rm ${docker ps -a -q}                | 删除所有容器                                       |
| docker rm -f <容器名或ID>                   | 删除某容器                                         |
| docker rmi -f <镜像名或ID>                  | 强制删除某镜像                                     |
| docker run -it -d -p --name                 | 运行一个容器,命名、设置端口映射、后台运行等        |
| docker save busy-box>/home/save.tar         | 保存镜像                                           |
| docker start container-name                 | 启动容器                                           |
| docker stop container-name                  | 停止容器                                           |
| docker tag                                  | 标记本地镜像                                       |
| docker unpause <docker name or id>          | 回复某一容器的所有进程                             |

### Docker Compose

> Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。



### Laradock

> https://laradock.io/

#### 安装配置

##### clone库

```
git clone https://github.com/Laradock/laradock.git
```

##### 配置文件

```
cp env-example .env
```

这个是用于指定你的应用程序的目录，默认是在 Laradock 的上一层。你可以根据需要修改为其它的位置，例如我的是：

```text
APPLICATION=../
APPLICATION=../app/
```

**记得最后要以 `/` 结尾。**

##### 构建容器

```
docker-compose up -d nginx mysql phpmyadmin redis workspace
```

#### PHP

**修改PHP版本**

修改.env配置文件版本

```text
PHP_VERSION=7.3
```

最后重建镜像

```bash
docker-compose build php-fpm
```

**修改PHP-CLI版本**

修改.env配置文件`PHP_VERSION`

```text
PHP_VERSION=7.3
```

最后重建图像，执行php-cli是在workspace镜像的容器中所以需要重新构建workspace

```bash
docker-compose build workspace
```

#### Nginx

##### 站点配置

- 进入nginx配置文件目录：`cd nginx/sites`

- 生成该站点的配置文件：`cp laravel.conf.example laradmin.conf`

- 配置该文件，相关配置参考nginx的配置：

  ```text
  server_name laradmin.test;
  # 开头必须是/var/www/，映射.env文件中APPLICATION配置对应的目录
  root /var/www/laradmin/public;
  ```

- 执行以下命令：

  ```
  docker-compose build nginx
  docker-compose restart nginx
  ```

- 修改 `/etc/hosts` 文件内容：`sudo vim /etc/hosts`

  ```
  127.0.0.1 laradmin.test
  ```

#### Mysql

默认情况下使用MySQL 8.0运行。其他版本：https://store.docker.com/images/mysql

- 修改.env laradock配置文件 `MYSQL_VERSION=5.7.31`
- 重新编译 `docker-compose build mysql`
- 如果已经运行则重新启动 `docker-compose restart mysql`

#### phpMyAdmin

`phpmyadmin`使用该`docker-compose up`命令运行phpMyAdmin

```text
docker-compose up -d mysql phpmyadmin
```

打开浏览器并访问端口**8080**上的localhost ： `http://localhost:8080`，登录信息如下

```text
host: mysql
user:	root
password:	root
```