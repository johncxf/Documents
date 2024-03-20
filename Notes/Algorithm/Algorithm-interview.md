# 算法笔记-面试

#### 合并两个有序数组

给出一个有序的整数数组 A 和有序的整数数组 B ，请将数组 B 合并到数组 A 中，变成一个有序的升序数组

数据范围： 0≤*n*, m≤100，|A_i| <=100， |B_i| <= 100
注意：

1. 保证 A 数组有足够的空间存放 B 数组的元素， A 和 B 中初始的元素数目分别为 m 和 n，A的数组空间大小为 m+n
2. 不要返回合并的数组，将数组 B 的数据合并到 A 里面就好了，且后台会自动将合并后的数组 A 的内容打印出来，所以也不需要自己打印
3. A 数组在[0,m-1]的范围也是有序的

```
// 输入：
[4,5,6],[1,2,3]
// 返回值：
[1,2,3,4,5,6]
// 说明：A数组为[4,5,6]，B数组为[1,2,3]，后台程序会预先将A扩容为[4,5,6,0,0,0]，B还是为[1,2,3]，m=3，n=3，传入到函数merge里面，然后请同学完成merge函数，将B的数据合并A里面，最后后台程序输出A数组

// 输入：
[1,2,3],[2,5,6]
// 返回值：
[1,2,2,3,5,6]
```

**解题思路**

双指针

倒序遍历，比较 A、B 中元素的大小，找到大的插入数组尾部，移动对应指针

**代码实现**

```go
/**
 * 合并两个有序数组
 *
 * @param A int 整型一维数组
 * @param B int 整型一维数组
 * @return void
 */
func mergeOrderlyArray(A []int, m int, B []int, n int) {
	if n == 0 {
		return
	}

	a, b := m-1, n-1
	for i := m + n - 1; i >= 0; i-- {
		if b < 0 || (a >= 0 && A[a] >= B[b]) {
			A[i] = A[a]
			a--
		} else {
			A[i] = B[b]
			b--
		}
	}
}
```

#### 反转字符串

将字符串中的所有单词进行反转，但是保留单词的顺序。例如，输入字符串为“Hello World”，则输出为“olleH dlroW"

解法1：

```go
func reverseString1(str string) string {
	// 先倒着遍历一遍
	arr := make([]string, len(str))
	for i := len(str) - 1; i >= 0; i-- {
		arr = append(arr, string(str[i]))
	}
    // 重新组装
	res := ""
	for _, v := range arr {
		res = res + v
	}
	return res
}
```

解法2:

```go
func reverseString2(str string) string {
	arr := []rune(str)
	for i, j := 0, len(arr)-1; i < j; i, j = i+1, j-1 {
		fmt.Println(arr[i])
		arr[i], arr[j] = arr[j], arr[i]
	}
	return string(arr)
}
```

#### 字符串中最长重复子串

给定一个字符串s，找出其中重复出现的非空子串的最大长度，即子串的长度大于1。

```
示例输入：s = "abcabcbb"
示例输出：3
```

**解题思路**

动态规划

```go
a b c a b c b b

```

代码：

```go
func lengthOfLongestSubstring(s string) int {
	n := len(s)
	max := 0
	// 构建二维数组
	dp := make([][]int, n+1)
	for i := range dp {
		dp[i] = make([]int, n+1)
	}
	for i := 1; i <= n; i++ {
		for j := i + 1; j <= n; j++ {
			if s[i-1] == s[j-1] {
				dp[i][j] = dp[i-1][j-1] + 1
				if dp[i][j] > max {
					max = dp[i][j]
				}
			}
		}
	}
	return max
}
```

