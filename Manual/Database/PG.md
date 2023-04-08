# PostgreSQL

学习文档：

- 官网：https://www.postgresql.org/
- 中文文档：http://www.postgres.cn/docs/12/

## 安装配置

### MacOS

#### HomeBrew 安装

安装:

```sh
# 查找 postgresql 可用版本
$ brew search postgresql

# 安装指定版本
$ brew install postgresql@15

# 安装默认版本
$ brew install postgresql
```

配置：

```sh
# 添加环境变量
$ echo 'export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc
$ source ~/.zshrc

# 查看版本
psql -V

# 启动/关闭/重启服务
$ brew services start postgresql@15
$ brew services stop postgresql@15
$ brew services restart postgresql@15
```

使用：

```sh
# 初始化（如果没有目录权限则先修改权限）
$ initdb /usr/local/var/postgres

# 配置环境遍历
export PGDATA=/usr/local/var/postgres
```

### Pgadmin

- https://www.pgadmin.org/download/pgadmin-4-macos/

## 基础使用

```sh
# 连接指定数据库
$ psql -h 127.0.0.1 -p 5432 -U xinfangchen -d postgres
```

### 常用命令

#### pgsql

```sql
# 查看所有用户
\du
\dg

# 查看所有数据库
\l

# 切换当前数据库
\c {dbname}

# 查看当前库下所有的表
\dt

# 查看指定表
\d {tablename}

# 查看数据目录
SHOW data_directory;

# 退出 sql
\q
```

#### SQL

用户操作：

```sql
# 创建用户
CREATE USER test WITH PASSWORD 'Test#1357';

# 修改密码
ALTER USER test WITH PASSWORD 'Test#2468';

# 指定用户添加指定角色
ALTER USER test createdb;

# 赋予指定账户指定数据库所有权限
GRANT ALL PRIVILEGES ON DATABASE test TO test;

# 移除指定账户指定数据库所有权限
REVOKE ALL PRIVILEGES ON DATABASE test FROM test;
```

数据库操作：

```sql
# 创建数据库
CREATE DATABASE test;

# 删除数据库
DROP DATABASE test;
```

数据表操作：

```sql
# 创建表
CREATE TABLE t1(id int,body varchar(100));

# 删除表
DROP TABLE t1;
```

CURD 操作：

```sql
# 插入一条记录
INSERT INTO t1(id, body) VALUES(1, 'Tom');

# 查询记录
SELECT * FROM t1 WHERE id = 1;

# 更新记录
UPDATE t1 SET body = 'Jack' WHERE id = 1;

# 删除指定的记录
DELETE FROM t1 WHERE id = 1;
```







