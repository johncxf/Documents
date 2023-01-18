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
