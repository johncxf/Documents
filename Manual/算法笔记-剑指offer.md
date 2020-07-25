## 剑指offer

### 简单

#### 1.构建乘积数组

##### 题目描述

> 给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2];）

##### 代码实现

```php
<?php

function multiply($numbers)
{
    $count = count($numbers);
    $arr = array();
    for ($i = 0; $i < $count; $i++) {
        $temp = 1;
        for ($j = 0; $j < $count; $j++) {
            if ($i != $j) {
                $temp *= $numbers[$j]; 
            }
        }
        $arr[$i] = $temp;
    }
    return $arr;
}
```

#### 2.不用加减乘除做加法

##### 题目描述

> 写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

##### 代码实现1

```php
<?php
function Add($num1, $num2)
{
    return array_sum(array(num1, num2));
}
```

##### 代码实现2

```php
<?php

function Add($num1, $num2)
{
    if($num1 == 0) return $num2;
    if($num2 == 0) return $num1;
    return Add($num1 ^ $num2, ($num1 & $num2) << 1);
}
```

##### 思路

转化成二进制进行计算，演示相关过程如下：

```
num1=2,num2=3
---
第一次：
2^3 : 0000 0010 ^ 0000 0011 = 0000 0001 = 1
2&3 : 0000 0010 & 0000 0011 = 0000 0010 = 2
2<<1: 0000 0010 << 1 = 0000 0100 = 4
第二次：
1和4进入
1^4 : 0000 0001 ^ 0000 0100 = 0000 0101 = 5
1&4 : 0000 0001 & 0000 0100 = 0000 0000 = 0
0<<1: 0000 0000 << 1 = 0000 0000 = 0
第三次：
5和0进入
返回5
```

###### 位运算符

| 例子           | 名称                | 结果                                                     |
| :------------- | :------------------ | :------------------------------------------------------- |
| **`$a & $b`**  | And（按位与）       | 将把 $a 和 $b 中都为 1 的位设为 1。                      |
| **`$a | $b`**  | Or（按位或）        | 将把 $a 和 $b 中任何一个为 1 的位设为 1。                |
| **`$a ^ $b`**  | Xor（按位异或）     | 将把 $a 和 $b 中一个为 1 另一个为 0 的位设为 1。         |
| **`~ $a`**     | Not（按位取反）     | 将 $a 中为 0 的位设为 1，反之亦然。                      |
| **`$a << $b`** | Shift left（左移）  | 将 $a 中的位向左移动 $b 次（每一次移动都表示“乘以 2”）。 |
| **`$a >> $b`** | Shift right（右移） | 将 $a 中的位向右移动 $b 次（每一次移动都表示“除以 2”）。 |

#### 3.

#### 1.二维数组中的查找

**题目描述**

> 在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**代码实现**

```php
<?php
function Find($target, $array)
{
    foreach($array as $key => $val){
        if(in_array($target, $val)){
            return "true";
        }
    }
    return "false";
}
while(fscanf(STDIN,"%d,%s",$target,$arr) == 2){
    eval('$array='.$arr.';');
    echo Find($target,$array)."\n";
}
```

#### 2.替换空格

**题目描述**

> 请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy

**代码实现**

```php
<?php
function replaceSpace($str)
{
    return str_replace(" ", "%20", $str);
}
```

#### 3.从头到尾打印链表

**题目描述**

> 输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

**代码实现**

php版

```php
<?php

/*class ListNode{
    var $val;
    var $next = NULL;
    function __construct($x){
        $this->val = $x;
    }
}*/
function printListFromTailToHead($head)
{
    $arr = [];
    $current = $head;
    while($current !== null){
        $arr[] = $current->val;
        $current = $current->next;
    }
    return array_reverse($arr);
}
```

python版

```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        list = []
        head = listNode
        while head:
            list.insert(0, head.val)
            head = head.next
        return list
```

