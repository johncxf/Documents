## GoLang

推荐文档：

- 入门文档：
  - https://laravelacademy.org/books/golang-tutorials
  - https://www.runoob.com/go/go-tutorial.html
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

### 字典

存储键值对映射关系的集合

#### 使用

##### 声明

```go
var testMap map[string]int
```

##### 初始化

```go
testMap := map[string]int{
  "one": 1,
  "two": 2,
  "three": 3,
}
```

##### 赋值

```go
testMap["four"] = 4
```

##### 删除元素

```go
delete(testMap, "four")
```

### 指针

变量的本质对一块内存空间的命名，我们可以通过引用变量名来使用这块内存空间存储的值，而指针则是用来指向这些变量值所在**内存地址的值**。

#### 初始化

当指针被声明后，没有指向任何变量内存地址时，它的零值是 nil，然后我们可以通过在给定变量前加上取地址符 `&` 获取该变量对应的内存地址，再将其赋值给声明的指针类型，这样，就完成对指针类型的初始化了，接下来我们可以通过在指针类型前加上间接引用符 `*` 获取指针指向内存空间存储的变量值。

```go
var ptr *int
fmt.Println(ptr)

a := 100
ptr = &a
fmt.Println(ptr)
fmt.Println(*ptr)
```

通过 `:=` 对指针类型进行初始化：

```go
a := 100
ptr := &a
fmt.Printf("%p\n", ptr)
fmt.Printf("%d\n", *ptr)
```

过内置函数 `new` 声明指针：

```go
ptr := new(int)
*ptr = 100
```

#### 通过指针传值

指针传值就类似于 PHP 中的引用传值，这样做的好处是节省内存空间，此外还可以在调用函数中实现对变量值的修改，因为直接修改的是指针指向内存地址上存储的变量值，而不是值拷贝

示例1：

```go
func swap(a, b int)  {
    a, b = b, a
    fmt.Println(a, b)
}

func main() {
    a := 1
    b := 2
    swap(a, b)
    fmt.Println(a, b)
}
```

输出：

```
2 1
1 2
```

示例2（使用指针）：

```go
func swap(a, b *int)  {
    *a, *b = *b, *a
    fmt.Println(*a, *b)
}

func main() {
    a := 1
    b := 2
    swap(&a, &b)
    fmt.Println(a, b)
}
```

输出：

```
2 1
2 1
```

因为这次，我们是通过指针传值的（`&a`、`&b` 都是指针，只不过我们没有显示声明而已），直接会对内存地址存储变量值进行交换操作，而主函数中的 `a`、`b` 变量仅仅是对应内存存储空间的别名而已，所以调用完 `swap` 函数后，它们所对应的内存空间存储值已经交换过来了。

## 流程控制

Golang支持如下几种流程控制语句：

- 条件语句：用于条件判断，对应的关键字有 `if`、`else` 和 `else if`；
- 分支语句：用于分支选择，对应的关键字有 `switch`、`case` 和 `select`（用于通道，后面介绍协程时会提到）；
- 循环语句：用于循环迭代，对应的关键字有 `for` 和 `range`；
- 跳转语句：用于代码跳转，对应的关键字有 `goto`。

### 条件语句

- if
- if...else...
- if...else if...else...

```go
package main

import "fmt"

func main() {
		score := 100
    if score > 90 {
        fmt.Println("Grade: A")
    } else if score > 80 {
        fmt.Println("Grade: B")
    } else if score > 70 {
        fmt.Println("Grade: C")
    } else if score > 60 {
        fmt.Println("Grade: D")
    } else {
        fmt.Println("Grade: F")
    }
   	fmt.Printf("a 的值为 : %d\n", a)
}
```

##### 注意事项

- 条件语句不需要使用圆括号将条件包含起来 `()`；
- 无论语句体内有几条语句，花括号 `{}` 都是必须存在的；
- 左花括号 `{` 必须与 `if` 或者 `else` 处于同一行；
- 在 `if` 后，条件语句前，可以添加变量初始化语句，使用 `;` 间隔，比如上述代码可以这么写 `if score := 100; score > 90 {`

### switch 语句

不需要在每个分支结构中显式通过 `break` 语句退出：

```go
score := 100
switch {
case score >= 90:
    fmt.Println("Grade: A")
case score >= 80 && score < 90:
    fmt.Println("Grade: B")
case score >= 70 && score < 80:
    fmt.Println("Grade: C")
case score >= 60 && score < 70:
    fmt.Println("Grade: D")
default:
    fmt.Println("Grade: F")
}
```

注：不能把变量 `score` 放到 `switch` 关键字后面，否则会报错，只有在与 `case` 分支值判等的时候，才可以这么做：

```go
score := 100
switch score {
case 90, 100:
    fmt.Println("Grade: A")
case 80:
    fmt.Println("Grade: B")
case 70:
    fmt.Println("Grade: C")
case 60:
case 65:
    fmt.Println("Grade: D")
default:
    fmt.Println("Grade: F")
}
```

#### 注意事项

- 和条件语句一样，左花括号 `{` 必须与 `switch` 处于同一行；
- 单个 `case` 中，可以出现多个结果选项（通过逗号分隔）；
- 与其它语言不同，Go 语言不需要用 `break` 来明确退出一个 `case`；
- 只有在 `case` 中明确添加 `fallthrough` 关键字，才会继续执行紧跟的下一个 `case`；
- 可以不设定 `switch` 之后的条件表达式，在这种情况下，整个 switch 结构与多个 `if...else...` 的逻辑作用等同。

### select 语句

```go
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

Go 语言中的循环语句只支持 `for` 关键字，而不支持 `while` 和 `do-while` 结构。关键字 `for` 的基本使用方法与其他语言类似，只是循环条件不含括号。

示例：

```go
sum := 0 
for i := 1; i <= 100; i++ { 
    sum += i 
}
fmt.Println(sum)
```

#### 无限循环

Go 语言不支持 `while` 和 `do-while` 循环语句，对于无限循环场景，可以通过不带循环条件的 `for` 语句实现：

```go
sum := 0
i := 0
for {
    i++
    if i > 100 {
        break
    }
    sum += i
}
fmt.Println(sum)
```

#### for-range 结构

```go
for k, v := range a {
    fmt.Println(k, v)
}
```

忽略索引

```go
for _, v := range a {
    fmt.Println(v)
}
```

忽略键值

```go
for k := range a {
    fmt.Println(k)
}
```

#### 注意事项

- 和条件语句、分支语句一样，左花括号 `{` 必须与 `for` 处于同一行；

- 不支持 `whie` 和 `do-while` 结构的循环语句；

- 可以通过 `for-range` 结构对可迭代集合进行遍历；

- 支持基于条件判断进行循环迭代；

- 允许在循环条件中定义和初始化变量，且支持多重赋值；

- Go 语言的 `for` 循环同样支持 `continue` 和 `break` 来控制循环，但是它提供了一个更高级的 `break`，可以选择中断哪一个循环，如下例：

  ```go
  JLoop: 
  for j := 0; j < 5; j++ { 
      for i := 0; i < 10; i++ { 
          if i > 5 { 
              break JLoop
          }
          fmt.Println(i)
      } 
  } 
  ```

  `break` 语句终止的是 `JLoop` 标签处的外层循环。

### 跳转语句

#### break和continue

golang中break和continue和其他语言一样，唯一不同之处是支持与标签结合跳转到指定的标签语句，从而改变这两个语句的默认跳转逻辑

```go
arr := [][]int{{1,2,3},{4,5,6},{7,8,9}}
ITERATOR1:
for i := 0; i < 3; i++ {
    for j := 0; j < 3; j++ {
        num := arr[i][j]
        if j > 1 {
            break ITERATOR1
        }
        fmt.Println(num)
    }
}
```

打印结果：

```
1
2
```

如果把 `break` 改成 `continue` 则打印结果如下：

```
1
2
4
5
7
8
```

#### goto

跳转到本函数内的某个标签，如：

```go
arr := [][]int{{1,2,3},{4,5,6},{7,8,9}}

for i := 0; i < 3; i++ {
    for j := 0; j < 3; j++ {
        num := arr[i][j]
        if j > 1 {
            goto EXIT
        }
        fmt.Println(num)
    }
}   

EXIT:
fmt.Println("Exit.")
```

打印：

```
1
2
Exit.
```

## 函数

### 函数入门使用

#### 函数定义

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

#### 函数调用

##### 同一包内调用：

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

##### 不同包内调用：

```go
package mymath

func Add(a, b int) int  {
    return a + b
}
```

```go
package main

import (
    "fmt"
    "mymath" //需要先导入了该函数所在的包
)

func main()  {
    fmt.Println(mymath.Add(1, 2))   // 3
}
```

**PS：在调用其他包定义的函数时，只有函数名首字母大写的函数才可以被访问**

Go 语言中没有 `public`、`protected`、`private` 之类的关键字，它是通过首字母的大小写来区分可见性的：首字母小写的函数只能在同一个包中访问，首字母大写的函数才可以在其他包中调用，Go 文件中定义的全局变量也是如此。

#### 函数返回值

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

#### 变长参数

##### 固定类型

只需要在参数类型前加上 `...` 前缀，就可以将该参数声明为变长参数：

```go
func myfunc(numbers ...int) {
    for _, number := range numbers {
        fmt.Println(number)
    }
}
```

函数 `myfunc()` 接受任意数量的参数，这些参数的类型全部是 `int`，所以它可以通过如下方式调用：

```go
myfunc(1, 2, 3, 4, 5) 
```

注：形如 `...type` 格式的类型只能作为函数的参数类型存在，并且必须是函数的最后一个参数。

##### 任意类型（泛型）

Go 语言也可以支持传递任意类型的值作为变长参数，指定变长参数类型为 `interface{}`，下面是 Go 语言标准库中 `fmt.Printf()` 的函数原型：

```go
func Printf(format string, args ...interface{}) { 
    // ...
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

### 匿名函数与闭包

#### 定义

所谓闭包指的是引用了自由变量（未绑定到特定对象的变量，通常在函数外定义）的函数，被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的上下文环境也不会被释放（比如传递到其他函数或对象中）。简单来说，「闭」的意思是「封闭外部状态」，即使外部状态已经失效，闭包内部依然保留了一份从外部引用的变量。

显然，闭包只能通过匿名函数实现，我们可以把闭包看作是**有状态的匿名函数**，反过来，如果匿名函数引用了外部变量，就形成了一个闭包（Closure）。

闭包的价值在于可以作为持有外部变量的函数对象或者匿名函数，对于类型系统而言，这意味着不仅要表示数据还要表示代码。支持闭包的语言都将函数作为**第一类对象**（firt-class object，有的地方也译作第一级对象、一等公民等，都是一个意思），Go 语言也不例外，这意味 Go 函数和普通 Go 数据类型（整型、字符串、数组、切片、字典、结构体等）具有同等的地位，可以赋值给变量，也可以作为参数传递给其他函数，还能够被函数动态创建和返回。

> 注：所谓第一类对象指的是运行期可以被创建并作为参数传递给其他函数或赋值给变量的实体，在绝大多数语言中，数值和基本类型都是第一类对象，在支持闭包的编程语言中（比如 Go、PHP、JavaScript、Python 等），函数也是第一类对象，而像 C、C++ 等不支持匿名函数的语言中，函数不能在运行期创建，所以在这些语言中，函数不是不是第一类对象。

```go
func(a, b int) int { 
    return a + b
}
```

#### 将匿名函数作为函数参数

匿名函数除了可以赋值给普通变量外，还可以作为参数传递到函数中进行调用，就像普通数据类型一样：

```go
add := func(a, b int) int {
    return a + b
}

// 将函数类型作为参数
func(call func(int, int) int) {
    fmt.Println(call(1, 2))
}(add)
```

当我们将函数声明数据类型时，需要严格指定每个参数和返回值的类型，这才是一个完整的函数类型，因此 `add` 函数对应的函数类型是 `func(int, int) int`。

也可以将第二个匿名函数提取到 `main` 函数外，成为一个具名函数 `handleAdd`，然后定义不同的加法算法实现函数，并将其作为参数传入 `handleAdd`：

```go
func main() {
    ...

    // 普通的加法操作
    add1 := func(a, b int) int {
        return a + b
    }

    // 定义多种加法算法
    base := 10
    add2 := func(a, b int) int {
        return a * base + b
    }

    handleAdd(1, 2, add1)
    handleAdd(1, 2, add2)
}

// 将匿名函数作为参数
func handleAdd(a, b int, call func(int, int) int) {
    fmt.Println(call(a, b))
}
```

#### 将匿名函数作为函数返回值

```go
// 将函数作为返回值类型
func deferAdd(a, b int) func() int {
    return func() int {
        return a + b
    }
}

func main() {
    ...

    // 此时返回的是匿名函数
    addFunc := deferAdd(1, 2)
    // 这里才会真正执行加法操作
    fmt.Println(addFunc())
}
```

#### 装饰器

装饰器模式（Decorator）是一种软件设计模式，其应用场景是为某个已经存在的功能模块（类或者函数）添加一些「装饰」功能，而又不会侵入和修改原有的功能模块。

Go 语言的设计哲学就是简单，没有提供「注解」之类的语法糖，在函数式编程中，要实现装饰器模式，可以借助高阶函数来实现。

```go
package main

import (
    "fmt"
    "time"
)

// 为函数类型设置别名提高代码可读性
type MultiPlyFunc func(int, int) int

// 乘法运算函数
func multiply(a, b int) int {
    return a * b
}

// 通过高阶函数在不侵入原有函数实现的前提下计算乘法函数执行时间
func execTime(f MultiPlyFunc) MultiPlyFunc {
    return func(a, b int) int {
        start := time.Now() // 起始时间
        c := f(a, b)  // 执行乘法运算函数
        end := time.Since(start) // 函数执行完毕耗时
        fmt.Printf("--- 执行耗时: %v ---\n", end)
        return c  // 返回计算结果
    }
}

func main() {
    a := 2
    b := 8
    // 通过修饰器调用乘法函数，返回的是一个匿名函数
    decorator := execTime(multiply)
    // 执行修饰器返回函数
    c := decorator(a, b)
    fmt.Printf("%d x %d = %d\n", a, b, c)
}
```

## 面向对象

Go 语言并没有沿袭传统面向对象编程中的诸多概念，比如类的继承、接口的实现、构造函数和析构函数、隐藏的 this 指针等，也没有 `public`、`protected`、`private` 之类的访问修饰符。

Go 语言对面向对象编程的支持是语言类型系统中的天然组成部分，整个类型系统通过接口串联，浑然一体。

### 类型系统

顾名思义，类型系统是指一个语言的类型体系结构。一个典型的类型系统通常包含如下基本内容：

- 基本类型，如 `byte`、`int`、`bool`、`float`、`string` 等；
- 复合类型，如数组、切片、字典、指针、结构体等；
- 可以指向任意对象的类型（Any 类型）；
- 值语义和引用语义；
- 面向对象，即所有具备面向对象特征（比如成员方法）的类型；
- 接口。

Go 语言中的大多数类型都是值语义，包括：

- 基本类型，如布尔类型、整型、浮点型、字符串等；
- 复合类型，如数组、结构体等（切片、字典、指针和通道都是引用语义）；

> 这里的值语义和引用语义等价于之前介绍类型时提到的值类型和引用类型。

### 类

#### 初始化

Go 语言的面向对象编程与我们之前所熟悉的 PHP、Java 那一套完全不同，没有 `class`、`extends`、`implements` 之类的关键字和相应的概念，而是借助**结构体**来实现类的声明，比如要定义一个学生类：

```go
type Student struct {
    id uint
    name string
    male bool
    score float64
}
```

Go 语言中也不支持构造函数、析构函数，取而代之地，可以通过定义形如 `NewXXX` 这样的全局函数（首字母大写）作为类的初始化函数：

```go
func NewStudent(id uint, name string, male bool, score float64) *Student {
    return &Student{id, name, male, score}
}
```

> 在 Go 语言中，未进行显式初始化的变量都会被初始化为该类型的零值，例如 `bool` 类型的零值为 `false`，`int` 类型的零值为 0，`string` 类型的零值为空字符串，`float` 类型的零值为 `0.0`。

#### 成员方法

##### 值方法

```go
func (s Student) GetName() string  {
    return s.name
}
```

##### 指针方法

```go
func (s *Student) SetName(name string) {
    s.name = name
}
```

我们可以把接收者类型为指针的成员方法叫做**指针方法**，把接收者类型为非指针的成员方法叫做**值方法**，二者的区别在于值方法传入的结构体变量是值类型（类型本身为指针类型除外），因此传入函数内部的是外部传入结构体实例的值拷贝，修改不会作用到外部传入的结构体实例。

#### 继承

Go 虽然没有直接提供继承相关的语法实现，但是我们通过**组合**的方式间接实现类似功能，所谓组合，就是将一个类型嵌入到另一个类型，从而构建新的类型结构。

> 传统面向对象编程中，显式定义继承关系的弊端有两个：一个是导致类的层级越来越复杂，另一个是影响了类的扩展性，很多软件设计模式的理念就是通过组合来替代继承提高类的扩展性。

Animal父类：

```go
type Animal struct {
    Name string
}

func (a Animal) Call() string {
    return "动物的叫声..."
}

func (a Animal) FavorFood() string {
    return "爱吃的食物..."
}

func (a Animal) GetName() string  {
    return a.Name
}
```

Dog 子类：

```go
type Dog struct {
    Animal
}
```

我们在 `Dog` 结构体类型中，嵌入了 `Animal` 这个类型，这样一来，我们就可以在 `Dog` 实例上访问所有 `Animal` 类型包含的属性和方法：

```go
func main() {
    animal := Animal{"中华田园犬"}
    dog := Dog{animal}

    fmt.Println(dog.GetName())
    fmt.Println(dog.Call())
    fmt.Println(dog.FavorFood())
}
```

上述代码的打印结果如下：

```
中华田园犬
动物的叫声...
爱吃的食物...
```

#### 方法重写

比如在上述 `Dog` 类型中，我们可以重写 `Call` 方法和 `FavorFood` 方法的实现如下：

```go
func (d Dog) FavorFood() string {
    return "骨头"
}

func (d Dog) Call() string {
    return "汪汪汪"
}
```

当然，你可以可以像这样继续调用父类 `Animal` 中的方法：

```go
fmt.Print(dog.Animal.Call())
fmt.Println(dog.Call())
fmt.Print(dog.Animal.FavorFood())
fmt.Println(dog.FavorFood())
```

#### 类属性和成员方法可见性

Go 语言基于包为单位组织和管理源码，因此变量、类属性、函数、成员方法的可见性都是基于包这个维度的。包与文件系统的目录结构存在映射关系（和命名空间一样）：

- 在引入 Go Modules 以前，Go 语言会基于 `GOPATH` 这个系统环境变量配置的路径为根目录（可能有多个），然后依次去对应路径下的 `src` 目录下根据包名查找对应的文件目录，如果目录存在，则再到该目录下的源文件中查找对应的变量、类属性、函数和成员方法；
- 在启用 Go Modules 之后，不再依赖 `$GOPATH` 定位包，而是基于 `go.mod` 中 `module` 配置值作为根路径，在该模块路径下，根据包名查找对应目录，如果存在，则继续到该目录下的源文件中查找对应变量、类属性、函数和成员方法。

在 Go 语言中，你可以通过 `import` 关键字导入官方提供的包、第三方包、以及自定义的包，导入第三方包时，还需要通过 `go get` 指令下载才能使用，如果基于 Go Modules 管理项目的话，这个依赖关系会自动维护到 `go.mod` 中。

归属同一个包的 Go 代码具备以下特性：

- 归属于同一个包的源文件包声明语句要一致，即同一级目录的源文件必须属于同一个包；
- 在同一个包下不同的源文件中不能重复声明同一个变量、函数和类（结构体）

##### 设置

Go 语言没有提供这些关键字，不管是变量、函数，还是自定义类的属性和成员方法，它们的可见性都是根据其首字母的大小写来决定的，如果变量名、属性名、函数名或方法名首字母大写，就可以在包外直接访问这些变量、属性、函数和方法，否则只能在包内访问，因此 Go 语言类属性和成员方法的可见性都是包一级的，而不是类一级的。

### 接口

在 Go 语言中，类对接口的实现和子类对父类的继承一样，并没有提供类似 `implement` 这种关键字显式声明该类实现了哪个接口，**一个类只要实现了某个接口要求的所有方法，我们就说这个类实现了该接口**。

#### 接口继承

Go 语言也支持类似的「接口继承」特性，但是由于不支持 `extends` 关键字，所以其实现和类的继承一样，是通过组合来完成。

```go
type A interface {
    Foo()
}

type B interface {
    A
    Bar()
}
```

## 异常处理机制

### error

Go 语言错误处理机制非常简单明了，不需要学习了解复杂的概念、函数和类型，Go 语言为错误处理定义了一个标准模式，即 `error` 接口，该接口的定义非常简单：

```go
type error interface { 
    Error() string 
}
```

#### 使用

将错误类型作为第二个参数返回

```go
func Foo(param int) (n int, err error) { 
    // ...
}
```

```go
n, err := Foo(0)

if err != nil { 
    // 错误处理 
} else {
    // 使用返回值 n 
}
```

#### 返回

通过 Go 标准错误包 `errors` 提供的 `New()` 方法快速创建一个 `error` 类型的错误实例：

```go
func add(a, b int) (c int, err error) {
    if (a < 0 || b < 0) {
        err = errors.New("只支持非负整数相加")
        return
    }
    a *= 2
    b *= 3
    c = a + b
    return
}
```

### defer

Go 语言中的类没有构造函数和析构函数的概念，处理错误和异常时也没有提供 `try...catch...finally` 之类的语法，那当我们想要在某个资源使用完毕后将其释放（网络连接、文件句柄等），或者在代码运行过程中抛出错误时执行一段兜底逻辑时，就可以通过 `defer` 关键字声明兜底执行或者释放资源的语句可以轻松解决这个问题。

- 一个函数/方法中可以存在多个 `defer` 语句
- `defer` 语句的调用顺序遵循先进后出的原则，即最后一个 `defer` 语句将最先被执行，相当于「栈」
- `defer` 后支持使用匿名函数来执行对应的兜底逻辑

示例：

```go
package main

import "fmt"

func printError()  {
    fmt.Println("兜底执行")
}

func main()  {
    defer printError()
    defer func() {
        fmt.Println("除数不能是0!")
    }()

    var i = 1
    var j = 1
    var k = i / j

    fmt.Printf("%d / %d = %d\n", i, j, k)
}
```

在函数正常执行的情况下，这两个 `defer` 语句会在最后一条打印语句执行完成后先执行第二条 `defer` 语句，再执行第一条 `defer` 语句

### panic

当代码运行时出错，而又没有在编码时显式返回错误时，Go 语言会抛出 panic，中文译作「运行时恐慌」，我们也可以将其看作 Go 语言版的异常。

```go
package main

import "fmt"

func main() {
    defer func() {
        fmt.Println("代码清理逻辑")
    }()

    var i = 1
    var j = 0
    if j == 0 {
        panic("除数不能为0！")
    }
    k := i / j
    fmt.Printf("%d / %d = %d\n", i, j, k)
}
```

`panic` 函数支持的参数类型是 `interface{}`，所以可以传入任意类型的参数：

```go
panic(500)   // 传入数字
panic(errors.New("除数不能为0"))  // 传入 error 类型
```

无论是 Go 语言底层抛出 panic，还是我们在代码中显式抛出 panic，处理机制都是一样的：当遇到 panic 时，Go 语言会中断当前协程（即 `main` 函数）后续代码的执行，然后执行在中断代码之前定义的 `defer` 语句（按照先入后出的顺序），最后程序退出并输出 panic 错误信息，以及出现错误的堆栈跟踪信息。

### recover

我们还可以通过 `recover()` 函数对 panic 进行捕获和处理，从而避免程序崩溃然后直接退出，而是继续可以执行后续代码，实现类似 Java、PHP 中 `try...catch` 语句的功能。

```go
package main

import (
    "fmt"
)

func divide() {
    defer func() {
        if err := recover(); err != nil {
            fmt.Printf("Runtime panic caught: %v\n", err)
        }
    }()

    var i = 1
    var j = 0
    k := i / j
    fmt.Printf("%d / %d = %d\n", i, j, k)
}

func main() {
    divide()
    fmt.Println("divide 方法调用完毕，回到 main 函数")
}
```

