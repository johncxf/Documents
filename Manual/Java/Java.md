# JAVA

教程：

- https://www.liaoxuefeng.com/wiki/1252599548343744

## 安装配置

### JDK 安装配置

#### 安装

mac&win&linux

- 下载对应版本JDK：https://www.oracle.com/java/technologies/downloads/
- 下载完成双击安装即可

#### 配置环境变量

##### mac

```shell
# 编辑 ~/.bash_profile 或者 ~/.zshrc
# JAVA_HOME 环境变量
export JAVA_HOME=`/usr/libexec/java_home -v 19`
# 把 JAVA_HOME 的 bin 目录附加到系统环境变量 PATH 上
export PATH=$JAVA_HOME/bin:$PATH
```

##### window

设置一个`JAVA_HOME`的环境变量（安装目录）

```shell
C:\Program Files\Java\jdk-19
```

把`JAVA_HOME`的`bin`目录附加到系统环境变量`PATH`上

```
Path=%JAVA_HOME%\bin;
```

### IDE 安装配置

#### Eclipse

安装：

- 下载安装包（Eclipse IDE for Java Developers）：https://www.eclipse.org/downloads/packages/
- 双击安装提示安装即可

## 基础语法

### 基本数据类型

- 整数类型：byte，short，int，long
- 浮点数类型：float，double
- 字符类型：char
- 布尔类型：boolean

#### 整型

- byte：-128 ~ 127
- short: -32768 ~ 32767
- int: -2147483648 ~ 2147483647
- long: -9223372036854775808 ~ 9223372036854775807

#### 浮点型

- float：3.4x1038
- double：1.79x10308

```java
float f1 = 3.14f;
float f2 = 3.14e38f; // 科学计数法表示的3.14x10^38
double d = 1.79e308;
double d2 = -1.79e308;
double d3 = 4.9e-324; // 科学计数法表示的4.9x10^-324
```

#### 布尔型

布尔类型`boolean`只有`true`和`false`两个值

```java
boolean b1 = true;
boolean b2 = false;
boolean isGreater = 5 > 3; // 计算结果为true
int age = 12;
boolean isAdult = age >= 18; // 计算结果为false
```

#### 字符型

字符类型`char`表示一个字符。Java的`char`类型除了可表示标准的ASCII外，还可以表示一个Unicode字符：

```java
char a = 'A';
char zh = '中';
```

注意`char`类型使用单引号`'`，且仅有一个字符，要和双引号`"`的字符串类型区分开。

#### 常量

定义变量的时候，如果加上`final`修饰符，这个变量就变成了常量

```java
final double PI = 3.14; // PI是一个常量
double r = 5.0;
double area = PI * r * r;
PI = 300; // compile error!
```

#### var 关键字

有些时候，类型的名字太长，写起来比较麻烦。例如：

```java
StringBuilder sb = new StringBuilder();
```

这个时候，如果想省略变量类型，可以使用`var`关键字：

```java
var sb = new StringBuilder();
```

编译器会根据赋值语句自动推断出变量`sb`的类型是`StringBuilder`。对编译器来说，语句：

```java
var sb = new StringBuilder();
```

实际上会自动变成：

```java
StringBuilder sb = new StringBuilder();
```

### 字符串类型

字符串类型`String`是引用类型，我们用双引号`"..."`表示字符串

```java
String s = ""; // 空字符串，包含0个字符
String s1 = "A"; // 包含一个字符
String s2 = "ABC"; // 包含3个字符
String s3 = "中文 ABC"; // 包含6个字符，其中有一个空格
```

转义字符：

- `\"` 表示字符`"`
- `\'` 表示字符`'`
- `\\` 表示字符`\`
- `\n` 表示换行符
- `\r` 表示回车符
- `\t` 表示Tab
- `\u####` 表示一个Unicode编码的字符

连接符：

java中使用`+`为连接符：

```java
String s1 = "Hello";
String s2 = "world";
String s = s1 + " " + s2 + "!";
System.out.println(s);
```

多行连接：

```java
// 使用 +
String s = "first line \n"
         + "second line \n"
         + "end";

// 使用 """..."""
String s = """
  SELECT * FROM
  users
  WHERE id > 100
  ORDER BY name DESC
  """;
```

### 数组类型

- 数组所有元素初始化为默认值，整型都是`0`，浮点型是`0.0`，布尔型是`false`；
- 数组一旦创建后，大小就不可改变。

定义数组：

```java
int[] ns = new int[5];
```

定义并初始化数组：

```java
int[] ns = new int[] { 68, 79, 91, 85, 62 };
```

简化：

```java
int[] ns = { 68, 79, 91, 85, 62 };
```

获取数组长度：

```java
int[] ns = new int[5];
System.out.println(ns.length); // 5
```

数组访问：

```java
int[] ns = { 68, 79, 91, 85, 62 };
System.out.println(ns[0]); // 68
```

#### 二维数组

```java
public class Main {
    public static void main(String[] args) {
        int[][] ns = {
            { 1, 2, 3, 4 },
            { 5, 6, 7, 8 },
            { 9, 10, 11, 12 }
        };
        System.out.println(ns.length); // 3
    }
}
```

#### 遍历数组

for：

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int i=0; i<ns.length; i++) {
            int n = ns[i];
            System.out.println(n);
        }
    }
}
```

for each

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}
```

### 流程控制

#### if判断

```java
if (条件) {
    // 条件满足时执行
} else if () {
  	//
} else {
  	//
}
```

#### switch

```java
switch (option) {
    case 1:
      System.out.println("Selected 1");
      break;
    case 2:
      System.out.println("Selected 2");
      break;
    default:
      System.out.println("Selected default");
      break;
}
```

#### while

```java
while (条件表达式) {
    循环语句
}
```

#### do while

```java
do {
    执行循环语句
} while (条件表达式);
```

#### for

```java
for (int i=0; i<5; i++) {
    System.out.println(i);
}
```

#### break&continue

在循环过程中，可以使用`break`语句跳出当前循环，`continue`则是提前结束本次循环，直接继续执行下次循环

## 面向对象



## 异常处理

```java
try {
    String s = processFile(“C:\\test.txt”);
    // ok:
} catch (FileNotFoundException e) {
    // file not found:
} catch (SecurityException e) {
    // no read permission:
} catch (IOException e) {
    // io error:
} catch (Exception e) {
    // other error:
} finally {
    // finally
}
```

#### 标准异常库

```shell
Exception
├─ RuntimeException
│  ├─ NullPointerException
│  ├─ IndexOutOfBoundsException
│  ├─ SecurityException
│  └─ IllegalArgumentException
│     └─ NumberFormatException
├─ IOException
│  ├─ UnsupportedCharsetException
│  ├─ FileNotFoundException
│  └─ SocketException
├─ ParseException
├─ GeneralSecurityException
├─ SQLException
└─ TimeoutException
```

## 日志

#### JDK Logging

Java 标准库内置的日志包

```java
import java.util.logging.Level;
import java.util.logging.Logger;

public class Hello {
    public static void main(String[] args) {
        Logger logger = Logger.getGlobal();
        logger.info("start process...");
        logger.warning("memory is running out...");
        logger.fine("ignored.");
        logger.severe("process will be terminated...");
    }
}
```

7个级别：

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

默认级别是INFO，INFO级别以下的日志，不会被打印出来

Logging系统在JVM启动时读取配置文件并完成初始化，一旦开始运行`main()`方法，就无法修改配置

#### Commons Logging

Commons Logging是一个第三方日志库，它是由Apache创建的日志模块

Commons Logging可以挂接不同的日志系统，并通过配置文件指定挂接的日志系统。默认情况下，Commons Loggin自动搜索并使用Log4j（Log4j是另一个流行的日志系统），如果没有找到Log4j，再使用JDK Logging。

##### 下载安装

- 下载并解压：https://commons.apache.org/proper/commons-logging/download_logging.cgi

##### 使用示例

```java
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Main {
    public static void main(String[] args) {
        Log log = LogFactory.getLog(Main.class);
        log.info("start...");
        log.warn("end.");
    }
}
```

- 找到`commons-logging-1.2.jar`这个文件，再把Java源码`Main.java`放到一个目录下
- 然后用`javac`编译`Main.java`，编译的时候要指定`classpath`：`javac -cp commons-logging-1.2.jar Main.java`
- 执行`Main.class`：`java -cp .:commons-logging-1.2.jar Main`

##### 日志级别

- FATAL
- ERROR
- WARNING
- INFO
- DEBUG
- TRACE

#### Log4j

Commons Logging，可以作为“日志接口”来使用。而真正的“日志实现”可以使用Log4j

## Maven

Maven是专门为Java项目打造的项目管理和构建工具，类似 NodeJS 的 NPM，PHP 的 composer。

官网：https://maven.apache.org/

它的主要功能有：

- 提供了一套标准化的项目结构；
- 提供了一套标准化的构建流程（编译，测试，打包，发布……）；
- 提供了一套依赖管理机制。

### 安装配置

#### MasOS

1、下载 `apache-maven-x.x.x-bin.tar.gz` 压缩包

- 下载地址：https://maven.apache.org/download.cgi

2、解压到自定义目录，如：`/usr/local`

3、配置环境变量

```sh
# 或者 ~/.bash_profile，根据自己使用终端决定
$ vim ~/.zshrc

# 添加环境变量（根据自己的目录进行修改）
export M2_HOME=/usr/local/apache-maven-3.9.1
export PATH=${PATH}:${M2_HOME}/bin

# 生效
$ source ~/.zshrc
```

4、查看是否安装成功

```sh
$ mvn -v

Apache Maven 3.9.1 (2e178502fcdbffc201671fb2537d0cb4b4cc58f8)
Maven home: /usr/local/apache-maven-3.9.1
Java version: 19.0.1, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-19.jdk/Contents/Home
Default locale: zh_CN_#Hans, platform encoding: UTF-8
OS name: "mac os x", version: "13.3", arch: "aarch64", family: "mac"
```

### 目录结构

一个使用Maven管理的普通的Java项目，它的目录结构默认如下：

```sh
./maven-project
├── pom.xml				// 项目描述文件
├── src
│   ├── main
│   │   ├── java		// 存放Java源码
│   │   └── resources	// 存放资源文件
│   └── test			// 单元测试
│       ├── java		// 单测源码
│       └── resources	// 单测资源
└── target
```

### pom.xml

```xml
<project ...>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.yiqiesuifeng.blog</groupId>
	<artifactId>hello</artifactId>
	<version>1.0</version>
	<packaging>jar</packaging>
	<properties>
        ...
	</properties>
	<dependencies>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
	</dependencies>
</project>
```

- `groupId`：类似于Java的包名，通常是公司或组织名称
- `artifactId`：类似于Java的类名，通常是项目名称
- `version`：项目版本号

一个Maven工程就是由`groupId`，`artifactId`和`version`作为唯一标识。我们在引用其他第三方库的时候，也是通过这3个变量确定。例如，依赖`commons-logging`：

```xml
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
```

使用`<dependency>`声明一个依赖后，Maven就会自动下载这个依赖包并把它放到classpath中。

### 依赖管理

Maven定义了几种依赖关系：

- `compile`：编译时需要用到该jar包（默认）
- `test`：编译Test时需要用到该jar包
- `runtime`：编译时不需要，但运行时需要用到
- `provided`：编译时需要用到，但运行时由JDK或某个服务器提供

使用示例：

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.3.2</version>
    <scope>test</scope>
</dependency>
```

### 镜像管理

#### 切换镜像

用户主目录下进入`.m2`目录，创建一个`settings.xml`配置文件，内容如下：

```xml
<settings>
    <mirrors>
        <mirror>
            <id>aliyun</id>
            <name>aliyun</name>
            <mirrorOf>central</mirrorOf>
            <!-- 阿里云的Maven镜像 -->
            <url>https://maven.aliyun.com/repository/central</url>
        </mirror>
    </mirrors>
</settings>
```

### 构建流程

Maven不但有标准化的项目结构，而且还有一套标准化的构建流程，可以自动化实现编译，打包，发布，等等。

- lifecycle相当于Java的package，它包含一个或多个phase；
- phase相当于Java的class，它包含一个或多个goal；
- goal相当于class的method，它其实才是真正干活的。

#### Lifecycle和Phase

Maven的生命周期（lifecycle）由一系列阶段（phase）构成

使用`mvn`这个命令时，后面的参数是phase，Maven自动根据生命周期运行到指定的phase。

常用命令：

- `mvn clean`：清理所有生成的class和jar
- `mvn clean compile`：先清理，再执行到`compile`
- `mvn clean test`：先清理，再执行到`test`，因为执行`test`前必须执行`compile`，所以这里不必指定`compile`
- `mvn clean package`：先清理，再执行到`package`

##### 生命周期`default`

它包含以下phase：

- validate
- initialize
- generate-sources
- process-sources
- generate-resources
- process-resources
- compile
- process-classes
- generate-test-sources
- process-test-sources
- generate-test-resources
- process-test-resources
- test-compile
- process-test-classes
- test
- prepare-package
- package
- pre-integration-test
- integration-test
- post-integration-test
- verify
- install
- deploy

运行`mvn package`，Maven就会执行`default`生命周期，它会从开始一直运行到`package`这个phase为止：

- validate
- ...
- package

运行`mvn compile`，Maven也会执行`default`生命周期，但这次它只会运行到`compile`，即以下几个phase：

- validate
- ...
- compile

##### 生命周期`clean`

它会执行3个phase：

- pre-clean
- clean （注意这个clean不是lifecycle而是phase）
- post-clean

#### Goal

执行一个phase又会触发一个或多个goal

- compile：compiler:compile
- test：
  - compiler:testCompile
  - surefire:test

goal的命名总是`abc:xyz`这种形式

### 模块管理

一个Maven大项目：

```sh
single-project
├── pom.xml
└── src
```

拆分成3个模块：

```sh
mutiple-project
├── pom.xml
├── parent
│   └── pom.xml		// 父级公共配置
├── module-a
│   ├── pom.xml
│   └── src
├── module-b
│   ├── pom.xml
│   └── src
└── module-c
    ├── pom.xml
    └── src
```

每个模块下面都有独立的 pom 配置，可以抽取pom公共配置作为父级配置，模块下面的pom配置基础父级配置

根目录下的 pom.xml 负责统一编译：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.practice.xxx</groupId>
    <artifactId>build</artifactId>
    <version>1.0</version>
    <packaging>pom</packaging>
    <name>build</name>

    <modules>
        <module>parent</module>
        <module>module-a</module>
        <module>module-b</module>
        <module>module-c</module>
    </modules>
</project>
```

父级 pom.xml 示例：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.practice.xxx</groupId>
    <artifactId>parent</artifactId>
    <version>1.0</version>
    <packaging>pom</packaging>

    <name>parent</name>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.28</version>
        </dependency>
        ...
    </dependencies>
</project>
```

注：parent的`<packaging>`是`pom`而不是`jar`，因为`parent`本身不含任何Java代码

module-a 的 pom.xml 示例：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.practice.xxx</groupId>
        <artifactId>parent</artifactId>
        <version>1.0</version>
        <relativePath>../parent/pom.xml</relativePath>
    </parent>

    <artifactId>module-a</artifactId>
    <packaging>jar</packaging>
    <name>module-a</name>
</project>
```

## Tomcat

### 安装

1、下载`tar.gz`压缩包

- https://tomcat.apache.org/download-10.cgi

2、解压到自定义目录，如：`~/Tools`

### 使用

进入 tomcat 根目录

```sh
# 启动服务
$ ./bin/startup.sh

# 关闭服务
$ ./bin/shutdown.sh
```

启动后浏览器访问：`http://localhost:8080/`





