# Mysql

## 安装配置

### MacOS

#### Brew 安装

```sh
# 查看可安装 mysql 版本
$ brew search mysql

# 执行安装
$ brew install mysql

# 启动 mysql 服务
$ brew services start mysql

# 配置 mysql 服务，过程中可以配置 root 密码
$ mysql_secure_installation

```

服务管理指令：

- `brew services start mysql`：启动 MySQL 服务器，并设置为自启动。
- `brew services stop mysql`：停止 MySQL 服务器，并设置为不自启动。
- `brew services run mysql`：只启动 MySQL 服务器。
- `mysql.server start`：启动 MySQL 服务器。
- `mysql.server stop`：停止 MySQL 服务器。

#### 下载安装

进入官方下载地址，选择合适的安装包下载安装即可（如m1芯片的下载 arm 处理器的 dmg 文件或者压缩包）

- https://dev.mysql.com/downloads/mysql/

下载后直接双击安装

### Linux

#### 源码安装

```shell
# 关闭 MySQL
${mysql_dir}/bin/mysqladmin -uroot -p shutdown
# 启动 MySQL的命令
${mysql_dir}/bin/mysqld_safe &
```

#### 密码重置

```shell
# 1、在配置文件中 [mysqld] 下添加 `skip-grant-tables`，保存退出（忘记密码情况）
# 2、重启 mysql服务
# 3、进入mysql执行： 
mysql > mysql -uroot -p
# 4、执行改密码
mysql > use mysql;
mysql >  update user set password=password('root') where user='root';   
mysql > flush privileges;
mysql > exit;
# 5、在配置文件中 [mysqld] 下删除 `skip-grant-tables`，保存退出
# 6、重启 mysql 服务
# 注意：以上只是一种修改mysql密码的方法，而且不适用于高版本 mysql
```

#### 开启远程访问

进入 mysql 命令行模式

```shell
mysql > use mysql;
mysql > update user set host='%' where host='localhost' and user='root';
mysql > grant all privileges on *.* to 'root'@'localhost' identified by 'root';
mysql > flush privileges;
mysql > exit
```

查看状态

```shell
# 显示*:3306则可以
$ ss -tlnp
```

### 连接工具

Mysql 连接管理工具：

- navicat：收费
  - 官网：https://www.navicat.com.cn/
- datagrip：收费
  - 下载地址：https://www.jetbrains.com/datagrip/
- phpadmin：免费，一个web服务，需要和php，nginx搭配使用，挺好用，用了很多年
  - 官网：https://www.phpmyadmin.net/

## 锁机制

### 全局锁

使用全局锁后整个数据库就处于只读状态，这时其他线程执行以下操作，都会被阻塞：

- 对数据的增删改操作，比如 insert、delete、update等语句；
- 对表结构的更改操作，比如 alter table、drop table 等语句。

全局锁主要应用于做**全库逻辑备份**，这样在备份数据库期间，不会因为数据或表结构的更新，而出现备份文件的数据与预期的不一样。

```mysql
# 上锁
> flush tables with read lock

# 解锁
> unlock tables
```

### 表级锁

#### 表锁

对学生表（t_student）加表锁：

```sh
# 表级别的共享锁，也就是读锁；
lock tables t_student read;

# 表级别的独占锁，也就是写锁；
lock tables t_stuent write;
```

如果本线程对学生表加了「共享表锁」，那么本线程接下来如果要对学生表执行写操作的语句，是会被阻塞的，当然其他线程对学生表进行写操作时也会被阻塞，直到锁被释放：

```sh
# 释放所有表锁
unlock tables
```

#### 元数据锁

