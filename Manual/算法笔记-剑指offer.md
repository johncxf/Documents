## 剑指offer

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

