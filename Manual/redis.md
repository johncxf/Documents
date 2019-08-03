---
typora-copy-images-to: ..\..\..\Image\markdown
---

#### redis安装

1. 官网下载redis安装包（https://redis.io/）

2. 将安装包上传至服务器

3. 解压安装包

4. 进入解压文件（redis-5.0.4）

5. 使用Make 编译源文件

   ```
   make
   ```

6. 安装到指定目录

   ```
   make PREFIX=/usr/local/redis install
   ```

7. 在/usr/local/redis下生成bin目录，下面有五个目录，则安装成功

   ![1554632484266](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1554632484266.png)

8. 将解压目录下的redis.conf配置文件复制到安转目录/usr/local/redis/下

9. 修改redis.conf，将daemonize no改为daemonize yes

   ![1554633072851](E:\Image\markdown\1554633072851.png)

#### redis使用

##### 启动服务

redis-server redis.conf（带目录）

```
/usr/local/redis/bin/redis-server /usr/local/redis/redis.conf
```

##### 查看服务

```
netstat -tunpl | grep 6379
```

![1554633282932](E:\Image\markdown\1554633282932.png)

##### 打开

```
./bin/redis-cli			--带路径
```

#### 字符串

| 命令   | 描述                              | 语法         |
| ------ | --------------------------------- | ------------ |
| set    | 设置键值                          | set age 20   |
| get    | 获取key对应键值                   | get age      |
| incr   | 对key值加加                       | incr age     |
| inrrby | 将 key 所储存的值加上给定的增量值 | incrby age 5 |

更多命令见：http://www.runoob.com/redis/redis-strings.html

#### 哈希

| 命令         | 描述                                                | 语法                             |
| ------------ | --------------------------------------------------- | -------------------------------- |
| hset         | 设置哈希                                            | hset key field value             |
| hmset        | 同时将多个 field-value (域-值)对设置到哈希表 key 中 | hmset key field1 value1 ...      |
| hsetnx       | 只有在字段 field 不存在时，设置哈希表字段的值       | hsetnx key field value           |
| hget         | 获取指定字段的值                                    | hget key field                   |
| hmget        | 获取所有给定字段的值                                | hmget key field1 value1 ...      |
| hgetall      | 获取在哈希表中指定 key 的所有字段和值               | hgetall key                      |
| hvals        | 获取哈希表中所有值                                  | hvals key                        |
| hdel         | 删除一个或多个哈希表字段                            | hdel key field1 ...              |
| hincrby      | 为哈希表 key 中的指定字段的整数值加上增量 increment | hincrby key field increment      |
| hincrbyfloat | 为哈希表中的字段值加上指定浮点数增量值              | hincrbyfloat key field increment |
| hexists      | 查看哈希表 key 中，指定的字段是否存在               | hexists key field                |
| hkeys        | 获取所有哈希表中的字段                              | hkeys keys                       |
| hlen         | 获取哈希表中字段的数量                              | hlen key                         |

#### 列表

> 具有栈（先进后出）和队列（先进先出）功能
>
> 添加元素：可以从头部或者尾部添加；
>
> 取出元素：从头部取出
>
> Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）
>
> 一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

| 命令   | 描述                         | 语法                  |
| ------ | ---------------------------- | --------------------- |
| lpush  | 将一个或多个值插入到列表头部 | lpush key value1 ...  |
| rpush  | 将一个或多个值插入到列表尾部 | rpush key value1 ...  |
| lrange | 获取列表指定范围内的元素     | lrange key start stop |

更多命令：http://www.runoob.com/redis/redis-lists.html

#### 集合

> Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。
>
> Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。
>
> 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)

| 命令     | 描述                     | 语法             |
| -------- | ------------------------ | ---------------- |
| sadd     | 向集合添加一个或多个成员 | sadd member1 ... |
| scard    | 获取集合的成员数         | scard key        |
| smembers | 返回集合中的所有成员     | smembers key     |

更多指令：http://www.runoob.com/redis/redis-sets.html

#### 有序集合

> Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。
>
> 不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。
>
> 有序集合的成员是唯一的,但分数(score)却可以重复。
>
> 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)

| 命令 | 描述                                                   | 语法                    |
| ---- | ------------------------------------------------------ | ----------------------- |
| zadd | 向有序集合添加一个或多个成员，或者更新已存在成员的分数 | zadd key score1 member1 |
|      |                                                        |                         |

