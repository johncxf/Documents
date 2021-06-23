## GoLang

推荐文档：

- 入门文档：https://laravelacademy.org/books/golang-tutorials
- 资源文档：https://geekr.dev/awesome-golang

## 环境安装

安装包下载地址：https://golang.google.cn/doc/install

### windows

1. 下载安装包（.msi）
2. 点击安装（选择安装位置）
3. 设置环境变量（默认设置好了）

### mac

#### HomeBrew 安装

```
brew install go
```

#### 下载安装

1. 下载安装包

2. 点击安装

3. 环境变量配置（默认 `GOPATH` 目录：`/Users/${username}/go/`）

   - 需要变更项目地址，编辑 `~/.bash_profile`文件

     ```
     # 项目目录
     export GOPATH=/Users/${username}/go/
     # 可执行文件目录
     export GOBIN=$GOPATH/bin
     # 将 go 可执行文件加入 PATH 中
     export PATH=$PATH:$GOBIN
     ```

   - 立即生效：`source ~/.bash_profile`

   - 注意：不要把`GOPATH`设置成`go`的安装路径

#### 配置

- 查看是否安装成功：`go version`
- 查看环境变量：`go env`

### IDE

- jetbrain golang
- vscode：需要安装golang扩展

## 开始使用

### 第一个程序

编写 `hello word`代码：

```go
package main

import "fmt"

func main() {
    fmt.Println("hello, world")
}
```

#### 编译运行

```go
go build hello.go
./hello
```

#### 直接运行

```go
go run hello.go
```

## 基础语法

#### 行分隔符

在 Go 程序中，一行代表一个语句结束。每个语句不需要像 C 家族中的其它语言一样以分号 ; 结尾，因为这些工作都将由 Go 编译器自动完成。

如果你打算将多个语句写在同一行，它们则必须使用 ; 人为区分，但在实际开发中我们并不鼓励这种做法。

#### 注释

单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释。多行注释也叫块注释，均已以 /* 开头，并以 */ 结尾。如：

```go
// 单行注释
/*
 Author
 我是多行注释
 */
```

#### 标识符

标识符用来命名变量、类型等程序实体。一个标识符实际上就是一个或是多个字母(A~Z和a~z)数字(0~9)、下划线_组成的序列，但是第一个字符必须是字母或下划线而不能是数字。

#### 字符串连接符

Go 语言的字符串可以通过 **+** 实现

#### 关键字

下面列举了 Go 代码中会使用到的 25 个关键字或保留字：

| break    | default     | func   | interface | select |
| -------- | ----------- | ------ | --------- | ------ |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

除了以上介绍的这些关键字，Go 语言还有 36 个预定义标识符：

| append | bool    | byte    | cap     | close  | complex | complex64 | complex128 | uint16  |
| ------ | ------- | ------- | ------- | ------ | ------- | --------- | ---------- | ------- |
| copy   | false   | float32 | float64 | imag   | int     | int8      | int16      | uint32  |
| int32  | int64   | iota    | len     | make   | new     | nil       | panic      | uint64  |
| print  | println | real    | recover | string | true    | uint      | uint8      | uintptr |

#### 空格

Go 语言中变量的声明必须使用空格隔开，如：

```
var age int;
```

## 数据类型

### 变量、常量

#### 变量

Go 语言变量名由字母、数字、下划线组成，其中首个字符不能为数字。

声明变量的一般形式是使用 var 关键字：

```
var identifier type
```

可以一次声明多个变量：

```
var identifier1, identifier2 type
```

示例：

```go
package main
import "fmt"
func main() {
    var a string = "Runoob"
    fmt.Println(a)

    var b, c int = 1, 2
    fmt.Println(b, c)
}
```

输出：

```
Runoob
1 2
```

##### 变量声明

**第一种，指定变量类型，如果没有初始化，则变量默认为零值**。

零值就是变量没有做初始化时系统默认设置的值。

```go
package main
import "fmt"
func main() {

  // 声明一个变量并初始化*
  var a = "RUNOOB"
  fmt.Println(a)

  // 没有初始化就为零值*
  var b int
  fmt.Println(b)

  // bool 零值为 false*
  var c bool
  fmt.Println(c)
}
```

以上实例执行结果为：

```
RUNOOB
0
false
```

- 数值类型（包括complex64/128）为 **0**

- 布尔类型为 **false**

- 字符串为 **""**（空字符串）

- 以下几种类型为 **nil**：

  ```
  var a *int
  var a []int
  var a map[string] int
  var a chan int
  var a func(string) int
  var a error // error 是接口
  ```

**第二种，根据值自行判定变量类型。**

```
var v_name = value
```

```go
package main
import "fmt"
func main() {
  var d = true
  fmt.Println(d)
}
```

输出结果是：

```
true
```

**第三种，省略 var, 注意 \**:=\** 左侧如果没有声明新的变量，就产生编译错误，格式：**

```
v_name := value
```

例如：

```go
var intVal int 

intVal :=1 // 这时候会产生编译错误

intVal,intVal1 := 1,2 // 此时不会产生编译错误，因为有声明新的变量，因为 := 是一个声明语句
```

可以将 var f string = "Runoob" 简写为 f := "Runoob"：

例：

```go
package main
import "fmt"
func main() {
    f := "Runoob" // var f string = "Runoob"
    fmt.Println(f)
}
```

输出结果是：

```
Runoob
```

##### 多变量声明

```go
//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3

var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断

vname1, vname2, vname3 := v1, v2, v3 // 出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误

// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)
```

##### 变量作用域

如果一个变量在函数体外声明，则被认为是全局变量，可以在整个包甚至外部包（变量名以大写字母开头）使用，不管你声明在哪个源文件里或在哪个源文件里调用该变量。在函数体内声明的变量称之为局部变量，它们的作用域只在函数体内，函数的参数和返回值变量也是局部变量。

#### 常量

##### 常量声明声明

常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

常量的定义格式：

```go
const identifier [type] = value
```

你可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型。

- 显式类型定义： `const b string = "abc"`
- 隐式类型定义： `const b = "abc"`

多个相同类型的声明可以简写为：

```go
const c_name1, c_name2 = value1, value2
```

##### 常量作用域

和函数体外声明的变量一样，以大写字母开头的常量在包外可见（类似于 `public` 修饰的类属性），比如上面介绍的 `Pi`、`Sunday` 等，而以小写字母开头的常量只能在包内访问（类似于通过 `protected` 修饰的类属性），比如 `zero`、`numberOfDays` 等

### 基本数据类型

#### 布尔类型

Go 语言中的布尔类型与其他主流编程语言差不多，类型关键字为 `bool`，可赋值且只可以赋值为预定义常量 `true` 和 `false`。

#### 整型

Go语言支持的整型类型非常丰富，你可以根据需要设置合适的整型类型，以节省内存空间。

| 类型      | 长度（单位：字节） | 说明                                 | 值范围                                   | 默认值 |
| --------- | ------------------ | ------------------------------------ | ---------------------------------------- | ------ |
| `int8`、  | 1                  | 带符号8位整型                        | -128~127                                 | 0      |
| `uint8`   | 1                  | 无符号8位整型，与 `byte` 类型等价    | 0~255                                    | 0      |
| `int16`   | 2                  | 带符号16位整型                       | -32768~32767                             | 0      |
| `uint16`  | 2                  | 无符号16位整型                       | 0~65535                                  | 0      |
| `int32`   | 4                  | 带符号32位整型，与 `rune` 类型等价   | -2147483648~2147483647                   | 0      |
| `uint32`  | 4                  | 无符号32位整型                       | 0~4294967295                             | 0      |
| `int64`   | 8                  | 带符号64位整型                       | -9223372036854775808~9223372036854775807 | 0      |
| `uint64`  | 8                  | 无符号64位整型                       | 0~18446744073709551615                   | 0      |
| `int`     | 32位或64位         | -                                    | -                                        | 0      |
| `uint`    | 32位或64位         | -                                    | -                                        | 0      |
| `uintptr` | 与对应指针相同     | 无符号整型，足以存储指针值的未解释位 | 32位平台下为4字节，64位平台下为8字节     | 0      |

#### 浮点型

浮点型也叫浮点数，用于表示包含小数点的数据，比如 `3.14`、`1.00` 都是浮点型数据。

Go 语言中的浮点数采用[IEEE-754](https://zh.wikipedia.org/zh-hans/IEEE_754) 标准的表达方式，定义了两个类型：`float32` 和 `float64`，其中 `float32` 是单精度浮点数，可以精确到小数点后 7 位（类似 PHP、Java 等语言的 `float` 类型），`float64` 是双精度浮点数，可以精确到小数点后 15 位（类似 PHP、Java 等语言的 `double` 类型）。

| 序号 | 类型和描述                        |
| :--- | :-------------------------------- |
| 1    | **float32** IEEE-754 32位浮点型数 |
| 2    | **float64** IEEE-754 64位浮点型数 |
| 3    | **complex64** 32 位实数和虚数     |
| 4    | **complex128** 64 位实数和虚数    |

------

#### 字符串类型

在 Go 语言中，字符串是一种基本类型，默认是通过 UTF-8 编码的字符序列，当字符为 ASCII 码时则占用 1 个字节，其它字符根据需要占用 2-4 个字节，比如中文编码通常需要 3 个字节。

##### 字符串声明

```go
var str string         // 声明字符串变量
str = "Hello World"    // 变量初始化
str2 := "Hello World"   // 也可以同时进行声明和初始化
```

##### 转义字符

Go 语言的字符串不支持单引号，只能通过双引号定义字符串字面值，如果要对特定字符进行转义，可以通过 `\` 实现，常见的需要转义的字符如下所示：

- `\n` ：换行符
- `\r` ：回车符
- `\t` ：tab 键
- `\u` 或 \U ：Unicode 字符
- `\\` ：反斜杠自身

类似 `node` ，也可以通过如下 ` 方式在字符串中包含双引号：

```go
str := `Search results for "Golang":`
```

##### 多行字符

多行字符可以使用 ` 获取 + 进行构建：

```go
results := `Search results for "Golang":
- Go
- Golang
Golang Programming
`
fmt.Printf("%s", results)
```

```go
results := "Search results for \"Golang\":\n" +
"- Go\n" +
"- Golang\n" +
"- Golang Programming\n"
fmt.Printf("%s", results)
```

**PS：在 Go 语言中，字符串是一种不可变值类型，一旦初始化之后，它的内容不能被修改。**

##### 字符串操作

###### 字符串连接

通过 + 号进行字符串连接

###### 字符串切片

在 Go 语言中，可以通过字符串切片实现获取子串的功能。

Go 切片区间可以对比数学中的区间概念来理解，它是一个**左闭右开**的区间，比如上述 `str[0:5]` 对应到字符串元素的区间是 `[0,5)`，`str[:5]` 对应的区间是 `[0,5)`（数组索引从 0 开始），`str[7:]` 对应的区间是 `[7:len(str)]`。

代码示例：

```go
str := "hello, world"
str1 := str[:5]  // 获取索引5（不含）之前的子串
str2 := str[7:]  // 获取索引7（含）之后的子串
str3 := str[0:5]  // 获取从索引0（含）到索引5（不含）之间的子串
fmt.Println("str1:", str1)
fmt.Println("str2:", str2)
fmt.Println("str3:", str3)
```

输出：

```go
str1: hello
str2: world
str3: hello
```

#### 其他数字类型

以下列出了其他更多的数字类型：

| 序号 | 类型和描述                               |
| :--- | :--------------------------------------- |
| 1    | **byte** 类似 uint8                      |
| 2    | **rune** 类似 int32                      |
| 3    | **uint** 32 或 64 位                     |
| 4    | **int** 与 uint 一样大小                 |
| 5    | **uintptr** 无符号整型，用于存放一个指针 |

#### 数据类型转化

由于 Go 是强类型语言，所以不支持动态语言那种自动转化，而是要对变量进行强制类型转化。

##### 数值类型之间的转化

调用要转化的数据类型对应的函数即可：

```go
v1 := uint(16)   // 初始化 v1 类型为 unit
v2 := int8(v1)   // 将 v1 转化为 int8 类型并赋值给 v2
v3 := uint16(v2) // 将 v2 转化为 uint16 类型并赋值给 v3
```

ps：在有符号与无符号以及高位数字向低位数字转化时，需要注意数字的溢出和截断。

##### 整型与浮点型之间的转化

- 浮点型转化为整型时，小数位被丢弃

  ```go
  v1 := 99.99
  v2 := int(v1)  // v2 = 99
  ```

- 整型转化为浮点型

  ```go
  v1 := 99
  v2 := float64(v2). // v2 = 99
  ```

##### 数值和布尔类型之间的转化

目前 Go 语言不支持将数值类型转化为布尔型，你需要自己根据需求去实现类似的转化。

##### 整型与字符串之间的转化

整型数据可以通过 Unicode 字符集转化为对应的 UTF-8 编码的字符串：

```go
v1 := 65
v2 := string(v1)  // v2 = A

v3 := 30028
v4 := string(v3)  // v4 = 界
```

### 数组

#### 数组定义

数组是所有语言编程中最常用的数据结构之一，Go 语言也不例外，与 PHP、JavaScript 等弱类型动态语言不同，在 Go 语言中，数组是固定长度的、同一类型的数据集合。数组中包含的每个数据项被称为数组元素，一个数组包含的元素个数被称为数组的长度。

在 Go 语言中，你可以通过 `[]` 来标识数组类型，但需要指定长度和元素类型。和普通变量赋值一样，数组也可以通过 `:=` 进行一次性声明和初始化，所有数组元素通过 `{}` 包裹，然后通过逗号分隔多个元素：

```go
[capacity]data_type{element_values}
```

示例：

```go
var a [8]byte // 长度为8的数组，每个元素为一个字节
var b [3][3]int // 二维数组（9宫格）
var c [3][3][3]float64 // 三维数组（立体的9宫格）
var d = [3]int{1, 2, 3}  // 声明时初始化
var e = new([3]string)   // 通过 new 初始化
```

#### 数组元素的访问和设置

##### 数组访问

通过数组下标访问

##### 数组设置

通过下标设置对应索引位置的元素值：

```
arr[0] = 100
```

### 切片

切片是一个新的数据类型，与数组最大的不同在于，切片的类型字面量中只有元素的类型，没有长度。

#### 创建切片

##### 直接创建

Go 语言提供的内置函数 `make()` 可以用于灵活地创建切片。

示例1：创建一个初始长度为 5 的整型切片：

```go
mySlice1 := make([]int, 5)
```

示例2：创建一个初始长度为 5、容量为 10 的整型切片（通过第三个参数设置容量）：

```go
mySlice2 := make([]int, 5, 10)
```

示例3：创建并初始化包含 5 个元素的数组切片（长度和容量均为5）：

```go
mySlice3 := []int{1, 2, 3, 4, 5}
```

和数组一样，所有未初始化的切片，会填充元素类型对应的零值。

事实上，使用直接创建的方式来创建切片时，Go 底层还是会有一个匿名数组被创建出来，然后调用基于数组创建切片的方式返回切片，只是上层不需要关心这个匿名数组的操作而已。所以，最终切片都是基于数组创建的，切片可以看做是操作数组的指针。

##### 基于数组

Go 语言支持通过 `array[start:end]` 这样的方式基于数组生成一个切片，`start` 表示切片在数组中的下标起点，`end` 表示切片在数组中的下标终点，两者之间的元素就是切片初始化后的元素集合，通过上面的示例可以看到，和字符串切片一样，这也是个**左闭右开**的集合

```go
// 先定义一个数组
months := [...]string{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}

// 基于数组创建切片
q2 := months[3:6]    // 第二季度
summer := months[5:8]  // 夏季

fmt.Println(q2)
fmt.Println(summer)  
```

##### 基于切片

类似于切片可以基于一个数组创建，切片也可以基于另一个切片创建：

```go
firsthalf := months[:6]
q1 := firsthalf[:3] // 基于 firsthalf 的前 3 个元素构建新切片
```

### 运算符

Go 语言内置的运算符有：

- 算术运算符
- 关系运算符
- 逻辑运算符
- 位运算符
- 赋值运算符
- 其他运算符

#### 算术运算符

下表列出了所有Go语言的算术运算符。假定 A 值为 10，B 值为 20。

| 运算符 | 描述 | 实例               |
| :----- | :--- | :----------------- |
| +      | 相加 | A + B 输出结果 30  |
| -      | 相减 | A - B 输出结果 -10 |
| *      | 相乘 | A * B 输出结果 200 |
| /      | 相除 | B / A 输出结果 2   |
| %      | 求余 | B % A 输出结果 0   |
| ++     | 自增 | A++ 输出结果 11    |
| --     | 自减 | A-- 输出结果 9     |

#### 关系运算符

下表列出了所有Go语言的关系运算符。假定 A 值为 10，B 值为 20。

| 运算符 | 描述                                                         | 实例              |
| :----- | :----------------------------------------------------------- | :---------------- |
| ==     | 检查两个值是否相等，如果相等返回 True 否则返回 False。       | (A == B) 为 False |
| !=     | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。   | (A != B) 为 True  |
| >      | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。   | (A > B) 为 False  |
| <      | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。   | (A < B) 为 True   |
| >=     | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 | (A >= B) 为 False |
| <=     | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 | (A <= B) 为 True  |

#### 逻辑运算符

下表列出了所有Go语言的逻辑运算符。假定 A 值为 True，B 值为 False。

| 运算符 | 描述                                                         | 实例               |
| :----- | :----------------------------------------------------------- | :----------------- |
| &&     | 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。 | (A && B) 为 False  |
| \|\|   | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。 | (A \|\| B) 为 True |
| !      | 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。 | !(A && B) 为 True  |

#### 位运算符

Go 语言支持的位运算符如下表所示。假定 A 为60，B 为13：

| 运算符 | 描述                                                         | 实例                                   |
| :----- | :----------------------------------------------------------- | :------------------------------------- |
| &      | 按位与运算符"&"是双目运算符。 其功能是参与运算的两数各对应的二进位相与。 | (A & B) 结果为 12, 二进制为 0000 1100  |
| \|     | 按位或运算符"\|"是双目运算符。 其功能是参与运算的两数各对应的二进位相或 | (A \| B) 结果为 61, 二进制为 0011 1101 |
| ^      | 按位异或运算符"^"是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。 | (A ^ B) 结果为 49, 二进制为 0011 0001  |
| <<     | 左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方。 其功能把"<<"左边的运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。 | A << 2 结果为 240 ，二进制为 1111 0000 |
| >>     | 右移运算符">>"是双目运算符。右移n位就是除以2的n次方。 其功能是把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数。 | A >> 2 结果为 15 ，二进制为 0000 1111  |

#### 赋值运算符

| 运算符 | 描述                                           | 实例                                  |
| :----- | :--------------------------------------------- | :------------------------------------ |
| =      | 简单的赋值运算符，将一个表达式的值赋给一个左值 | C = A + B 将 A + B 表达式结果赋值给 C |
| +=     | 相加后再赋值                                   | C += A 等于 C = C + A                 |
| -=     | 相减后再赋值                                   | C -= A 等于 C = C - A                 |
| *=     | 相乘后再赋值                                   | C *= A 等于 C = C * A                 |
| /=     | 相除后再赋值                                   | C /= A 等于 C = C / A                 |
| %=     | 求余后再赋值                                   | C %= A 等于 C = C % A                 |
| <<=    | 左移后赋值                                     | C <<= 2 等于 C = C << 2               |
| >>=    | 右移后赋值                                     | C >>= 2 等于 C = C >> 2               |
| &=     | 按位与后赋值                                   | C &= 2 等于 C = C & 2                 |
| ^=     | 按位异或后赋值                                 | C ^= 2 等于 C = C ^ 2                 |
| \|=    | 按位或后赋值                                   | C \|= 2 等于 C = C \| 2               |

#### 其他运算符

| 运算符 | 描述             | 实例                       |
| :----- | :--------------- | :------------------------- |
| &      | 返回变量存储地址 | &a; 将给出变量的实际地址。 |
| *      | 指针变量。       | *a; 是一个指针变量         |

### 条件语句

#### if 语句

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 10
 
   /* 使用 if 语句判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" )
   }
   fmt.Printf("a 的值为 : %d\n", a)
}
```

#### if else 语句

```go
package main

import "fmt"

func main() {
   /* 局部变量定义 */
   var a int = 100;
 
   /* 判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" );
   } else {
       /* 如果条件为 false 则执行以下语句 */
       fmt.Printf("a 不小于 20\n" );
   }
   fmt.Printf("a 的值为 : %d\n", a);

}
```

#### switch 语句

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var grade string = "B"
   var marks int = 90

   switch marks {
      case 90: grade = "A"
      case 80: grade = "B"
      case 50,60,70 : grade = "C"
      default: grade = "D"  
   }

   switch {
      case grade == "A" :
         fmt.Printf("优秀!\n" )     
      case grade == "B", grade == "C" :
         fmt.Printf("良好\n" )      
      case grade == "D" :
         fmt.Printf("及格\n" )      
      case grade == "F":
         fmt.Printf("不及格\n" )
      default:
         fmt.Printf("差\n" );
   }
   fmt.Printf("你的等级是 %s\n", grade );      
}
```

#### select 语句

```
package main

import "fmt"

func main() {
   var c1, c2, c3 chan int
   var i1, i2 int
   select {
      case i1 = <-c1:
         fmt.Printf("received ", i1, " from c1\n")
      case c2 <- i2:
         fmt.Printf("sent ", i2, " to c2\n")
      case i3, ok := (<-c3):  // same as: i3, ok := <-c3
         if ok {
            fmt.Printf("received ", i3, " from c3\n")
         } else {
            fmt.Printf("c3 is closed\n")
         }
      default:
         fmt.Printf("no communication\n")
   }    
}
```

### 循环语句

#### for 循环

Go 语言的 For 循环有 3 种形式，只有其中的一种使用分号。

和 C 语言的 for 一样：

```go
for init; condition; post { }
```

**例：**

```go
package main

import "fmt"

func main() {
        sum := 0
        for i := 0; i <= 10; i++ {
                sum += i
        }
        fmt.Println(sum)
}
```

**输出：**

> 55

和 C 的 while 一样：

```go
for condition { }
```

**例：**

```go
package main

import "fmt"

func main() {
        sum := 1
        for ; sum <= 10; {
                sum += sum
        }
        fmt.Println(sum)

        // 这样写也可以，更像 While 语句形式
        for sum <= 10{
                sum += sum
        }
        fmt.Println(sum)
}
```

> 16
>
> 16

和 C 的 for(;;) 一样：

```
for { }
```

**例：**

```go
package main

import "fmt"

func main() {
        sum := 0
        for {
            sum++ // 无限循环下去
        }
        fmt.Println(sum) // 无法输出
}
```

- init： 一般为赋值表达式，给控制变量赋初值；
- condition： 关系表达式或逻辑表达式，循环控制条件；
- post： 一般为赋值表达式，给控制变量增量或减量。

### 函数

#### 定义

格式如下：

```
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

函数定义解析：

- func：函数由 func 开始声明
- function_name：函数名称，函数名和参数列表一起构成了函数签名。
- parameter list：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
- return_types：返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
- 函数体：函数定义的代码集合。

**例：**

```go
/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 声明局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result 
}
```

#### 调用

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int = 200
   var ret int

   /* 调用函数并返回最大值 */
   ret = max(a, b)

   fmt.Printf( "最大值是 : %d\n", ret )
}

/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 定义局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result 
}
```

#### 返回值

go语言函数可以return多个值

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("Google", "Runoob")
   fmt.Println(a, b)
}
```

#### 引用传值

**例：**

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int= 200

   fmt.Printf("交换前，a 的值 : %d\n", a )
   fmt.Printf("交换前，b 的值 : %d\n", b )

   /* 调用 swap() 函数
   * &a 指向 a 指针，a 变量的地址
   * &b 指向 b 指针，b 变量的地址
   */
   swap(&a, &b)

   fmt.Printf("交换后，a 的值 : %d\n", a )
   fmt.Printf("交换后，b 的值 : %d\n", b )
}

func swap(x *int, y *int) {
   var temp int
   temp = *x    /* 保存 x 地址上的值 */
   *x = *y      /* 将 y 值赋给 x */
   *y = temp    /* 将 temp 值赋给 y */
}
```

**输出：**

```go
交换前，a 的值 : 100
交换前，b 的值 : 200
交换后，a 的值 : 200
交换后，b 的值 : 100
```

