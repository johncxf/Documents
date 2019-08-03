### 面试中遇到的题

#### SQL查询优化

1. 尽量避免在列上进行运算，这样会导致索引失效 `select * from t where YEAR(d) >= 2011` 优化为：`select * from t where`

2. 使用join时，应该用小结果集驱动大结果集。同事把复杂的JOIN查询拆分成多个Query.使用JOIN多个表时，可能导致更多的锁定和堵塞。

3. 注意LIKE模糊查询的使用，避免%%

    `select * from t where name like '%de%'` 优化为: `select * from t where name > '%de%' AND name < 'df'`

4. 仅列出需要查询的字段，这对速度不会有明显影响，主要考虑节省内存。避免使用 select *

5. 使用批量插入语句节省交互

6. limit的基础比较大时使用between

    `select * from article order by id limit 10000000, 10` 优化为： `select * from article where id between 10000000 and 10000010 order by id`

    between限定比limit夸，所以海量数据访问时，用between或者where替换掉里面。 id不能有断行啊。

7. 不要使用rand函数获取多条随机记录， 可先生成随机数去查询

8. 避免使用null

9. 不要使用count(id), 而应该使用count(*)

10. 不要做无谓的排序操作，而应该竟可能在索引中完成排序

#### include、include_once...区别

了解下include、include_once、require和require_once这4个函数：

- include函数：会将指定的文件读入并且执行里面的程序；
- require函数：会将目标文件的内容读入，并且把自己本身代换成这些读入的内容；
- include_once 函数：在脚本执行期间包含并运行指定文件。此行为和 include 语句类似，唯一区别是如果该文件中已经被包含过，则不会再次包含。如同此语句名字暗示的那样，只会包含一次；
- require_once 函数：和 require 语句完全相同，唯一区别是 PHP 会检查该文件是否已经被包含过，如果是则不会再次包含。

include在引入不存文件时产生一个警告且脚本还会继续执行，而require则会导致一个致命性错误且脚本停止执行。

#### 类的自动加载

sql_autoload_register()

> 可以注册任意数量的自动加载器，当使用尚未被定义的类（class）和接口（interface）时自动去加载。通过注册自动加载器，脚本引擎在 PHP 出错失败前有了最后一个机会加载所需的类

> 尽管 autoload()函数也能自动加载类和接口，但更建议使用 spl_autoload_register()函数。 spl_autoload_register() 提供了一种更加灵活的方式来实现类的自动加载（同一个应用中，可以支持任意数量的加载器，比如第三方库中的）。因此，不再建议使用 autoload()函数，在以后的版本中它可能被弃用。

#### 命名空间

> 什么是命名空间？从广义上来说，命名空间是一种封装事物的方法

PHP 命名空间可以解决以下两类问题：

1. 用户编写的代码与PHP内部的类/函数/常量或第三方类/函数/常量之间的名字冲突。
2. 为很长的标识符名称(通常是为了缓解第一类问题而定义的)创建一个别名（或简短）的名称，提高源代码的可读性。

#### Redis数据类型

> Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

#### js跨域

jsonp跨域

CORS跨域

#### HTTP状态码



#### HTTP请求方式

> get、post、put、delete、head、options、trace
>

#### 排序

> 多数情况自己写一个排序实现数组的排序

#### linux常用指令



#### php常用函数



#### php魔术方法



#### 提取一段url中的参数

> 写一个函数提取一段url中的参数

#### php-fpm

> PHP-FPM(PHP FastCGI Process Manager)意：PHP FastCGI 进程管理器，用于管理PHP 进程池的软件，用于接受web服务器的请求
>
> PHP-FPM提供了更好的PHP进程管理方式，可以有效控制内存和进程、可以平滑重载PHP配置。



#### sql实现建表以及查询

> 写SQL语句建表
>
> 写查询语句：联合查询

#### session和cookie知识点

> 两者区别
>
> 项目中如何使用

#### 看一段代码写出结果

> 引用
>
> $a++,++$a
>
> 不同数据类型变量间比较

#### 哔哩哔哩面试

**视频面之前做的题**

> 一个长度为N的整数数组(N>2)，找出其中相乘最大的2个数。如Arr=[1,2,3],则最大值为2X3=6
> 函数定义：int fun(Arr){return 相乘最大的2个数;}

**笔试题一**

> 笔试题一：有一个从小到大排列的整数数组A，元素个数是6万。在A中查找某个整数n，如果n存在于A中则返回它的下标，否则返回-1，你能想到几种查找办法？
> 测试用例：[-1, 2, 9, 100, 200]-->查找2返回1，查找3返回-1

**笔试题二**

> 有一个整数数组B，将数组中的0移到数组最后，要求不改变其他数字排序，不额外申请内存空间。
> 测试用例：[-1, 0, 1, 0, 4] --> [-1, 1, 4, 0, 0]
> 测试用例：[0, 0, -1, 0, 1, 0, 4] --> [-1, 1, 4, 0, 0, 0]
> 测试用例：[-1, 0, 1, 0, 4, 0, 0] --> [-1, 1, 4, 0, 0, 0, 0]

**答辩题**

> 用户无法访问哔哩哔哩网站，如何排查这个问题？
>
> apache和nginx的区别
>
> nginx如何配置
>
> 为什么需要nginx，只有php不是也可以运行

#### 用户空间和系统空间

> 用户态和内核态
>
> 用户空间就是用户进程所在的内存区域，相对的，系统空间就是操作系统占据的内存区域。用户进程和系统进程的所有数据都在内存中
>
> <https://blog.csdn.net/qq_34228570/article/details/72995997>

#### 微服务

> 微服务架构是一种将单应用程序作为一套小型服务开发的方法，每种应用程序都在其自己的进程中运行，并与轻量级机制（通常是HTTP资源的API）进行通信

#### 计算机网络

> 三次握手四次挥手
>
> OSI七层模型
>
> tcp处于哪一层
>
> HTTP

#### MyISAM与InnoDB两者之间区别？



#### mysql有什么索引？

#### 一般你会怎么建索引？

#### 写出常见的十个php函数？

















### PHP面试知识点

#### 1.引用变量

**例题：**

什么是引用变量？在php中，用什么符号表示？

> 在php中引用意味着用不同的名字访问同一个变量内容，用&符号

**引用变量的工作原理**

```php
<?php
$a = range(0, 1000);//定义变量，开辟一个a的存储空间
var_dump(memory_get_usage());
$b = $a;//定义b，将a赋值给b，此时不会重新开辟空间
var_dump(memory_get_usage());
$a = range(0, 1000);//对a进行修改，开辟空间变化
var_dump(memory_get_usage());
```

![1560753307369](../Image/markdown/1560753307369.png)

```php
<?php
$a = range(0, 1000);//定义变量，开辟一个a的存储空间
var_dump(memory_get_usage());
$b = &$a;//$a,$b都指向$a
var_dump(memory_get_usage());
$a = range(0, 1000);//对a进行修改，开辟空间变化不大
var_dump(memory_get_usage());
```

![1560752789933](../Image/markdown/1560752789933.png)

**unset删除引用**

![1560753482200](../Image/markdown/1560753482200.png)

**注意：对象本身就是引用传递**

**面试题**

![1560754175983](../Image/markdown/1560754175983.png)

#### 2.常量及数据类型

例题：php字符串的定义方式及各自的区别

定义方式：

- 单引号：不能解析变量，不能解析转义字符，效率更高
- 双引号：可以解析变量和所有转义字符
- heredoc：类似双引号

![1560945611922](../Image/markdown/1560945611922.png)

- newdoc：类似单引号

![1560945620827](../Image/markdown/1560945620827.png)

##### **2.1数据类型**

> 八大数据类型可以归为三大数据类型

**标量数据类型**

- 浮点类型
- 布尔类型

false的七种情况：

![1560945883197](../Image/markdown/1560945883197.png)

![1560945909744](../Image/markdown/1560945909744.png)

- 数组类型

![1560946014861](../Image/markdown/1560946014861.png)

$_SERVER:

<https://www.cnblogs.com/rendd/p/6182918.html>

![1560946222980](../Image/markdown/1560946222980.png)

##### **2.2常量**

![1560946275704](../Image/markdown/1560946275704.png)

![1560946308956](../Image/markdown/1560946308956.png)

![1560946327877](../Image/markdown/1560946327877.png)

#### 3.运算符

**例题：**

foo()和@foo()之间的区别

== 和 === 的区别

示例：0 == 'abc'

> 数字和字符串比较，PHP会把字符串转换成数字再进行比较
>
> PHP转换的规则的是：**若字符串以数字开头，则取开头数字作为转换结果，若无则输出0**

![1560947968223](../Image/markdown/1560947968223.png)

#### 4.流程控制

![1560948227199](../Image/markdown/1560948227199.png)

![1560948340153](../Image/markdown/1560948340153.png)

![1560948387345](../Image/markdown/1560948387345.png)

![1560948680451](../Image/markdown/1560948680451.png)

#### 5.自定义函数和内部函数

##### 5.1变量

![1560992542690](../Image/markdown/1560992542690.png)

![1560992603130](../Image/markdown/1560992603130.png)

![1560992751158](../Image/markdown/1560992751158.png)

![1560992968484](../Image/markdown/1560992968484.png)

![1560994555083](../Image/markdown/1560994555083.png)

![1560994604553](../Image/markdown/1560994604553.png)

![1560994640502](../Image/markdown/1560994640502.png)

##### 2.内置函数

![1560994909971](../Image/markdown/1560994909971.png)

![1560994928530](../Image/markdown/1560994928530.png)

![1560994939963](../Image/markdown/1560994939963.png)

![1560994960441](../Image/markdown/1560994960441.png)

![1560994994332](../Image/markdown/1560994994332.png)

![1560995033012](../Image/markdown/1560995033012.png)

例题：

![1560995437809](../Image/markdown/1560995437809.png)

#### 6.正则表达式

基础

后向引用

贪婪模式

常见的PCRE函数

![1561021119081](../Image/markdown/1561021119081.png)

中文匹配

![1561021127142](../Image/markdown/1561021127142.png)

#### 7.文件目录操作

##### fopen()函数

> fopen() 函数打开文件或者 URL
>
> 如果打开失败，本函数返回 FALSE

```
fopen(filename,mode,include_path,context)
```

| 参数         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| filename     | 必需。规定要打开的文件或 URL。                               |
| mode         | 必需。规定要求到该文件/流的访问类型。可能的值见下表。        |
| include_path | 可选。如果也需要在 include_path 中检索文件的话，可以将该参数设为 1 或 TRUE。 |
| context      | 可选。规定文件句柄的环境。Context 是可以修改流的行为的一套选项。 |

**mode 参数的可能的值**

| mode | 说明                                                         |
| :--- | :----------------------------------------------------------- |
| "r"  | 只读方式打开，将文件指针指向文件头。                         |
| "r+" | 读写方式打开，将文件指针指向文件头。                         |
| "w"  | 写入方式打开，将文件指针指向文件头并将文件大小截为零。如果文件不存在则尝试创建之。 |
| "w+" | 读写方式打开，将文件指针指向文件头并将文件大小截为零。如果文件不存在则尝试创建之。 |
| "a"  | 写入方式打开，将文件指针指向文件末尾。如果文件不存在则尝试创建之。 |
| "a+" | 读写方式打开，将文件指针指向文件末尾。如果文件不存在则尝试创建之。 |
| "x"  | 创建并以写入方式打开，将文件指针指向文件头。如果文件已存在，则 fopen() 调用失败并返回 FALSE，并生成一条 E_WARNING 级别的错误信息。如果文件不存在则尝试创建之。这和给底层的 open(2) 系统调用指定 O_EXCL\|O_CREAT 标记是等价的。此选项被 PHP 4.3.2 以及以后的版本所支持，仅能用于本地文件。 |
| "x+" | 创建并以读写方式打开，将文件指针指向文件头。如果文件已存在，则 fopen() 调用失败并返回 FALSE，并生成一条 E_WARNING 级别的错误信息。如果文件不存在则尝试创建之。这和给底层的 open(2) 系统调用指定 O_EXCL\|O_CREAT 标记是等价的。此选项被 PHP 4.3.2 以及以后的版本所支持，仅能用于本地文件。 |

**读取文件**

##### fread()

```
fread(file,length)
```

> fread() 函数读取文件（可安全用于二进制文件）

| 参数   | 描述                           |
| :----- | :----------------------------- |
| file   | 必需。规定要读取打开文件。     |
| length | 必需。规定要读取的最大字节数。 |

##### fgetc()

```
fgetc(file)
```

fgetc() 函数从文件指针中读取一个字符

##### fgets()

```
fgets(file,length)
```

> fgets() 函数从文件指针中读取一行。

| 参数   | 描述                                         |
| :----- | :------------------------------------------- |
| file   | 必需。规定要读取的文件。                     |
| length | 可选。规定要读取的字节数。默认是 1024 字节。 |

##### fclose()

```
fclose(file)
```

> fclose() 函数关闭一个打开文件

##### file_get_contents()

> file_get_contents() 函数把整个文件读入一个字符串中

```
file_get_contents(path,include_path,context,start,max_length)
```

| 参数         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| path         | 必需。规定要读取的文件。                                     |
| include_path | 可选。如果也想在 include_path 中搜寻文件的话，可以将该参数设为 "1"。 |
| context      | 可选。规定文件句柄的环境。context 是一套可以修改流的行为的选项。若使用 null，则忽略。 |
| start        | 可选。规定在文件中开始读取的位置。该参数是 PHP 5.1 新加的。  |
| max_length   | 可选。规定读取的字节数。该参数是 PHP 5.1 新加的。            |

##### file_put_contents()

> file_put_contents() 函数把一个字符串写入文件中。
>
> 与依次调用 fopen()，fwrite() 以及 fclose() 功能一样。

```
file_put_contents(file,data,mode,context)
```

| 参数    | 描述                                                         |
| :------ | :----------------------------------------------------------- |
| file    | 必需。规定要写入数据的文件。如果文件不存在，则创建一个新文件。 |
| data    | 可选。规定要写入文件的数据。可以是字符串、数组或数据流。     |
| mode    | 可选。规定如何打开/写入文件。可能的值：FILE_USE_INCLUDE_PATHFILE_APPENDLOCK_EX |
| context | 可选。规定文件句柄的环境。context 是一套可以修改流的行为的选项。若使用 null，则忽略。 |

##### file()

> file() 函数把整个文件读入一个数组中。
>
> 与 file_get_contents()类似，不同的是 file() 将文件作为一个数组返回。数组中的每个单元都是文件中相应的一行，包括换行符在内。
>
> 如果失败，则返回 false。

```
file(path,include_path,context)
```

##### readfile()

> readfile() 函数输出一个文件。
>
> 该函数读入一个文件并写入到输出缓冲。
>
> 若成功，则返回从文件中读入的字节数。若失败，则返回 false。您可以通过 @readfile() 形式调用该函数，来隐藏错误信息。

```
readfile(filename,include_path,context)
```

##### 例题：

![1563502546543](../Image/markdown/1563502546543.png)

![1563502431174](../Image/markdown/1563502431174.png)

![1563502561813](../Image/markdown/1563502561813.png)

![1563504771562](../Image/markdown/1563504771562.png)

#### 8.会话控制

##### cookie

> 存储在客户端中的文件

**setcookie**

```
setcookie(name,value,expire,path,domain,secure)
```

| 参数   | 描述                                               |
| :----- | :------------------------------------------------- |
| name   | 必需。规定 cookie 的名称。                         |
| value  | 必需。规定 cookie 的值。                           |
| expire | 可选。规定 cookie 的有效期。                       |
| path   | 可选。规定 cookie 的服务器路径。                   |
| domain | 可选。规定 cookie 的域名。                         |
| secure | 可选。规定是否通过安全的 HTTPS 连接来传输 cookie。 |

**$_COOKIE**

> 读取

**优缺点**

> 保存在客户端，不会占用服务器资源，同时不安全

##### session

> session称为会话信息，位于web服务器上

**操作**

```
session_start();
$_SESSION;
$_SESSION = [];
session_destroy();
```

**配置**



##### session与cookie的区别

（1）Cookie以文本文件格式存储在浏览器中，而session存储在服务端它存储了限制数据量。它只允许4kb它没有在cookie中保存多个变量。

（2）cookie的存储限制了数据量，只允许4KB，而session是无限量的

（3）我们可以轻松访问cookie值但是我们无法轻松访问会话值，因此它更安全

（4）设置cookie时间可以使cookie过期。但是使用session-destory（），我们将会销毁会话。

#### 9.面向对象

##### 继承

> 单一继承
>
> 方法重写

##### 多态

> 抽象类定义
>
> 接口定义

##### 魔术方法

![1563524614613](../Image/markdown/1563524614613.png)

##### 设计模式

#### 10.网络协议

##### HTTP状态码

##### OSI七层模型

##### HTTP协议

![1563525505953](../Image/markdown/1563525505953.png)

![1563526761557](../Image/markdown/1563526761557.png)

##### HTTP请求方法

![1563526876295](../Image/markdown/1563526876295.png)

##### HTTPS

![1563526918722](../Image/markdown/1563526918722.png)

#### 11.JavaScript和jQuery

#### 12.AJAX

#### 13.Linux

##### Linux常用命令

**系统安全**

> sudo	su	chmod	setfacl

**进程管理**

> w	top	ps	kill	pkill	pstree	killall

**用户管理**

> id	usermod	useradd	groupadd	userdel

**文件系统**

> mount	umount	fsck	df	du

**系统关机重启**

> shutdown	reboot

**网络应用**

> curl	telnet	mail	elinks

**网络测试**

> ping	netstat	host

**网络配置**

> hostname	ifconfig

**常用工具**

> ssh	screen	clear	who	date

**软件包管理**

> yum	rpm	apt-get

**文件查找**

> locate	find

......

##### 系统定时任务

> crontab -e
>
> at

##### vi/vim编译器

一般模式、编辑模式、命令模式、视图模式

![1564734527472](../Image/markdown/1564734527472.png)

##### shell基础

![1564734643989](../Image/markdown/1564734643989.png)

![1564734681037](../Image/markdown/1564734681037.png)

![1564734734899](../Image/markdown/1564734734899.png)

![1564734782387](../Image/markdown/1564734782387.png)

#### 14.mysql

##### 数据类型

##### 基础操作

##### 存储引擎

##### 锁机制

##### 事务处理、存储过程、触发器