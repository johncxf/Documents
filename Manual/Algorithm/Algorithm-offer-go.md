# 剑指offer-golang

## 入门

#### [递归] 斐波那契数列

##### 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0，第1项是1）。

n<=39

##### 代码实现

**递归**

```go
/**
 *递归
 *
 * @param n int整型
 * @return int整型
 */
func Fibonacci( n int ) int {
	// 边界值处理
	if n == 0 || n == 1 {
		return n
	}
	return Fibonacci(n -1) + Fibonacci(n - 2)
}
```

##### 说明

斐波那契数列指的是这样一个数列：0、1、1、2、3、5、8、13、21、34、……在数学上，斐波那契数列以如下被以递推的方法定义：*F*(0)=0，*F*(1)=1, *F*(n)=*F*(n - 1)+*F*(n - 2)（*n* ≥ 3，*n* ∈ N*）

## 简单

#### [递归] 跳台阶

##### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）

##### 代码实现

**递归**

```go
/**
 * 递归
 *
 * @param number int整型 
 * @return int整型
*/
func jumpFloor( number int ) int {
    if number == 1 || number == 2 {
        return number
    }
    return jumpFloor (number - 1) +  jumpFloor (number - 2)
}
```

##### 思路

由题意可知此题和斐波那契数列解法相同

#### [数学] 跳台阶扩展问题

##### 描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶(n为正整数)总共有多少种跳法。

##### 示例

```
输入：3
输出：4
```

##### 代码实现

```go
/**
 * 2^（n - 1）
 * 
 * @param number int整型 
 * @return int整型
*/
func jumpFloorII( number int ) int {
    return 1 << (number - 1)
}
```

##### 思路

由题意可知结果为2的n-1次方，所以可以转化为一个数学问题

#### [数组] 数组中重复的数字

##### 描述

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任一一个重复的数字。 例如，如果输入长度为7的数组[2,3,1,0,2,5,3]，那么对应的输出是2或者3。存在不合法的输入的话输出-1

##### 示例

输入：

```
[2,3,1,0,2,5,3]
```

返回值：

```
2
```

说明：

```
2或3都是对的 
```

##### 代码实现

```go
/**
 * 【暴力】数组中重复的数字
 *
 * @param numbers int整型一维数组
 * @return int整型
 */
func duplicate(numbers []int) int {
	ret := -1
	// 遍历数组
	for i := 0; i < len(numbers); i++ {
		//fmt.Println("Element", i, "of arr is", numbers[i])
		for j := 0; j < len(numbers); j++ {
			if i == j {
				continue
			}
			if numbers[i] == numbers[j] {
				ret = numbers[i]
			}
		}
	}
	return ret
}
```

#### [数组] 构建乘积数组

##### 题目描述

给定一个数组 A[0,1,...,n-1]，请构建一个数组 B[0,1,...,n-1]，其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2]）

对于A长度为1的情况，B无意义，故而无法构建，因此该情况不会存在。

##### 示例

```
输入：
[1,2,3,4,5]
返回值：
[120,60,40,30,24]
```

##### 代码实现

时间复杂度：O(n^2)

```go
/**
 * [暴力] 构建乘积数组
 *
 * @param A int整型一维数组
 * @return int整型一维数组
 */
func multiply(A []int) []int {
	n := len(A)
	B := make([]int, n)
	for i := 0; i < n; i++ {
		B[i] = 1;
		for j := 0; j < n; j++ {
			if i == j {
				continue
			}
			B[i] *= A[j]
		}
	}
	return B
}
```

**优化方法**

时间复杂度：O(n)

```go
/**
 * 构建乘积数组
 *
 * @param A int整型一维数组
 * @return int整型一维数组
 */
func multiply(A []int) []int {
	n := len(A)
	letf := 1
	B := make([]int, n)
	for i, v := range A {
		B[i] = letf
		letf *= v
	}
	right := 1
	for j := n - 1; j >= 0; j-- {
		B[j] *= right
		right *= A[j]
	}
	return B
}
```

#### [数学]不用加减乘除做加法

##### 题目描述

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

##### 示例

```
输入：
1,2
返回值：
3
```

##### 代码实现1

```go
/**
 * 不用加减乘除做加法
 *
 * @param num1 int整型
 * @param num2 int整型
 * @return int整型
 */
func Add(num1 int, num2 int) int {
	for num2 != 0 {
		c := (num1 & num2) << 1		//进位
		num1 ^= num2				//非进位和
		num2 = c
	}
	return num1
}
```

##### 思路

可以使用位运算符求解

#### [字符串] 替换空格

##### 描述

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

##### 示例

输入：

```
"We Are Happy"
```

返回值：

```
"We%20Are%20Happy"
```

##### 代码实现

```go
/**
 * 替换字符串
 *
 * @param s string字符串
 * @return string字符串
 */
func replaceSpace(s string) string {
	res := ""
	for _, v := range s {
		if v == ' ' {
			res += "%20"
		} else {
			res += string(v)
		}
	}
	return res
}
```

#### [字符串] 第一个只出现一次的字符串

##### 描述

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符，并返回它的位置，如果没有则返回 -1（需要区分大小写）.（从0开始计数）

##### 示例

输入：

```
"google"
```

返回值：

```
4
```

##### 代码实现

```go
/**
 * 第一个只出现一次的字符串
 *
 * @param str string字符串
 * @return int整型
 */
func FirstNotRepeatingChar(str string) int {
	if len(str) == 0 {
		return -1
	}
	keyMap := [256]int {0}
	for i := range str {
		keyMap[str[i]]++
	}
	for i , v := range str {
		if keyMap[v] == 1 {
			return i
		}
	}
	return -1
}
```

##### 思路

先开一个map数组统计字符串每个元素出现的次数，再遍历字符串，如果map中的某个元素值为1，代表出现1次，直接返回

