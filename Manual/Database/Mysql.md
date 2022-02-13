# Mysql

## 安装配置

### 启动服务

**源码安装**

```shell
//关闭MySQL
${mysql_dir}/bin/mysqladmin -uroot -p shutdown
//启动MySQL的命令
${mysql_dir}/bin/mysqld_safe &
```

### 密码重置

在配置文件中 [mysqld] 下添加 `skip-grant-tables`，保存退出（忘记密码情况）
重启 mysql服务
进入mysql `mysql -uroot -p`

执行改密码

```shell
mysql > use mysql;
mysql >  update user set password=password('root') where user='root';   
mysql > flush privileges;
mysql > exit;
```

在配置文件中 [mysqld] 下删除 `skip-grant-tables`，保存退出

重启 mysql 服务

**注意：**以上只是一种修改mysql密码的方法，而且不适用与高版本mysql

### 开启远程访问

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
ss -tlnp
显示*:3306则可以
```
