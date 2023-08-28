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

### MacOS 多版本共存

1、下载多个版本JDK进行安装：https://www.oracle.com/java/technologies/downloads/

```sh
# 查看已安装JDK：
$ /usr/libexec/java_home -V

# 或通过查看 /Library/Java/JavaVirtualMachines 目录下存在对应多个版本JDK
$ ls /Library/Java/JavaVirtualMachines
# 输出：jdk-17.jdk jdk-19.jdk
```

2、编辑 `~/.bash_profile` 或者 `~/.zshrc`（如果之前配置过 JAVA_HOME 配置记得删除）

```sh
# Java config
export JAVA_17_HOME="/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home"
export JAVA_19_HOME="/Library/Java/JavaVirtualMachines/jdk-19.jdk/Contents/Home"

# config alias
alias jdk17="export JAVA_HOME=$JAVA_17_HOME"
alias jdk19="export JAVA_HOME=$JAVA_19_HOME"

# config default jdk
export JAVA_HOME=$JAVA_19_HOME
export PATH="$JAVA_HOME:$PATH"
```

3、保存生效配置信息

```sh
$ source ~/.bash_profile

# ~/.zshrc 对应
$ source ~/.zshrc
```

4、切换 JDK 版本

```sh
# 查看默认版本
$ java -version
java version "19.0.1" 2022-10-18
Java(TM) SE Runtime Environment (build 19.0.1+10-21)
Java HotSpot(TM) 64-Bit Server VM (build 19.0.1+10-21, mixed mode, sharing)

# 切换成 JDK17 版本
$ jdk17
$ java -version
java version "17.0.8" 2023-07-18 LTS
Java(TM) SE Runtime Environment (build 17.0.8+9-LTS-211)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.8+9-LTS-211, mixed mode, sharing)
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

## 注解

注解（Annotation）是放在Java源码的类、方法、字段、参数前的一种特殊“注释”

注释会被编译器直接忽略，注解则可以被编译器打包进入class文件，因此，注解是一种用作标注的“元数据”。

### 注解的作用

注解可以分为三类：

1、内置注解（由编译器使用的注解），如：

- `@Override`：让编译器检查该方法是否正确地实现了覆写；
- `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。

2、由工具处理`.class`文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。

3、在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。

### 注解的定义

#### 元注解

有一些注解可以修饰其他注解，这些注解就称为元注解（meta annotation）。Java标准库已经定义了一些元注解，我们只需要使用元注解，通常不需要自己去编写元注解。

##### @Target

定义注解能够被应用于源码的哪些位置

```java
// 定义注解 @Report 可用在方法上
@Target(ElementType.METHOD)
public @interface MyAnnotation1 {
    int type() default 0;
    String level() default "info";
    String value() default "";
}

// 定义注解 @Report 可用在方法或字段上
@Target({
    ElementType.METHOD,
    ElementType.FIELD
})
public @interface MyAnnotation2 {
    ...
}
```

##### @Retention

定义注解的生命周期，默认为 `CLASS`，使用最多一般是 `RUNTIME`

- 仅编译期：`RetentionPolicy.SOURCE`
- 仅class文件：`RetentionPolicy.CLASS`
- 运行期：`RetentionPolicy.RUNTIME`

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

##### @Repeatable

定义注解是否可重复

```java
@Repeatable(Reports.class)
@Target(ElementType.TYPE)
public @interface MyAnnotation {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

##### @Inherited

定义子类是否可继承父类定义的注解

```java
@Inherited
@Target(ElementType.TYPE)
public @interface MyAnnotation {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

#### 定义注解

格式：

```java
public @interface xxx {...}
```

1. 用 `@interface` 定义注解
2. 添加参数、默认值
3. 用元注解配置注解

```java
// 用元注解配置注解
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
// 用 @interface 定义注解
public @interface MyAnnotation {
    // 注解的参数
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

- 定义一个注解时，还可以定义配置参数。配置参数可以包括：基本类型、String、Class、枚举类型。
- 可以使用 default 来声明参数的默认值
- 最常用的参数定义为`value()`，推荐所有参数都尽量设置默认值
- 必须设置`@Target`和`@Retention`，`@Retention`一般设置为`RUNTIME`，便于运行期读取该注解

## 反射

Java的反射是指程序在运行期可以拿到一个对象的所有信息

## 泛型



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

## MyBatis

文档：https://mybatis.org/mybatis-3/zh/index.html

## Swagger

官网：https://swagger.io/

### SpringBoot 使用 Swagger

springBoot3.x 不支持 swagger

引入依赖：

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>3.0.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>3.0.0</version>
</dependency>
```

新建配置：

新建配置：

xxx.xxx/config/SwaggerConfig.java

```java
package com.yiqiesuifeng.config;

import org.springframework.context.annotation.Configuration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
// 开启 Swagger
@EnableSwagger2
public class SwaggerConfig {
	// 进行配置
}
```

访问：http://localhost:8081/swagger-ui.html

## SpringDoc

文档：https://springdoc.org/

由于 swagger 不再更新，目前也不支持 springboot3.x，目前取而代之的就是 springDoc

引入依赖：

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.1.0</version>
</dependency>
```

新建配置：

xxx.xxx/config/OpenApiConfig.java

```java
package com.yiqiesuifeng.config;

import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import org.springdoc.core.models.GroupedOpenApi;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class OpenApiConfig {
    /**
     * SpringDoc 配置
     *
     * @return openApi 配置信息
     */
    @Bean
    public OpenAPI springDocOpenAPI() {
        return new OpenAPI().info(new Info()
                        .title("API DOCUMENT")
                        .description("接口文档")
                        .version("0.0.1-SNAPSHOT"));
    }

    /**
     * 分组配置
     *
     * @return 分组接口
     */
    @Bean
    public GroupedOpenApi siteApi() {
        return GroupedOpenApi.builder()
                .group("Hello")
                .pathsToMatch("/hello/**")
                .build();
    }
}
```





















