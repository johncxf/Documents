# 算法 - 剑指offer

## 数据结构

### 链表

#### [JZ6-简单] 从尾到头打印链表

##### 描述

输入一个链表的头节点，按链表从尾到头的顺序返回每个节点的值（用数组返回）。

如输入{1,2,3}的链表如下图:

![image-202107222309](../../Image/image-202107222309.png)

返回一个数组为 [3,2,1]

0 <= 链表长度 <= 1000

##### 示例

输入：

```
// 输入
{1,2,3}
//返回值
[3,2,1]

// 输入
{67,0,24,58}
// 返回
[58,24,0,67]
```

##### 解题思路

先遍历链表一次存入数组中，再倒序处理数组

##### 代码实现

Golang

```go
// 定义节点
type ListNode struct {
	Val int
	Next *ListNode
}

/**
 * 从尾到头打印链表
 *
 * @param head ListNode类
 * @return int整型一维数组
 */
func printListFromTailToHead(head *ListNode) []int {
    // 定义切片
	ret := make([]int, 0)
    // 遍历链表
	for head != nil {
        // 将链表值插入切片数组头部
		ret = append([]int{head.Val}, ret...)
		head = head.Next
	}

	return ret
}
```

#### [JZ22-简单] 链表中倒数最后k个结点

##### 描述

输入一个链表，输出一个链表，该输出链表包含原链表中从倒数第k个结点至尾节点的全部节点。

如果该链表长度小于k，请返回一个长度为 0 的链表。

##### 示例

```go
// 输入
{1,2,3,4,5}, 1 

// 返回
{5} 
```

##### 代码实现

**方法一：**

1. 先遍历统计链表长度，记为 n；
2. 设置一个指针走 (n-k) 步，即可找到链表倒数第 k个节点。

```go
/**
 * 链表中倒数最后k个结点
 *
 * @param pHead ListNode类
 * @param k int整型
 * @return ListNode类
 */
func findKthToTail(pHead *ListNode,  k int) *ListNode {
	current, res := pHead, pHead
    n := 0
    for current != nil {
		current = current.Next
		n++
	}
    if (k > n) {
        return nil
    }
	// current 指针先走 k 步
    for i := 1; i <= (n - k); i++ {
		res = res.Next
	}
	return res
}
```

**方法二（快慢指针）：**

初始化： 当前指针 current 、后指针 prev ，双指针都指向头节点 head 
构建双指针距离： current 指针先向前走 k 步（结束后，双指针 current 和 prev 间相距 k 步）
双指针共同移动： 循环中，current 和 prev 每轮都向前走一步，直至 current 走过链表 尾节点 时跳出（跳出后， prev 与尾节点距离为 k-1，即 prev 指向倒数第 k 个节点）。
返回值： 返回 prev 即可。

```go
/**
 * 链表中倒数最后k个结点
 *
 * @param pHead ListNode类
 * @param k int整型
 * @return ListNode类
 */
func FindKthToTail(pHead *ListNode,  k int) *ListNode {
	// 边界值
	if pHead == nil || k <= 0 {
		return nil
	}

	current, prev := pHead, pHead

	// current 指针先走 k 步
	for i := 0; i < k; i++ {
		if current == nil {
			return nil
		}
		current = current.Next
	}

	// 双指针每轮都向前走一步，直至 current 走过链表 尾节点 时跳出
	// 跳出后，prev 与尾节点距离为 k-1，即 prev 指向倒数第 k 个节点
	for current != nil {
		current = current.Next
		prev = prev.Next
	}
	return prev
}
```

#### [JZ25-简单] 合并两个排序的链表

##### 描述

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

##### 示例

```
// 输入
{1,3,5},{2,4,6}
// 返回值
{1,2,3,4,5,6}
```

##### 代码实现

**方法一（递归）：**

```go
/**
 * 合并两个排序的链表（递归）
 *
 * @param pHead1 ListNode类
 * @param pHead2 ListNode类
 * @return ListNode类
 */
func Merge(pHead1 *ListNode , pHead2 *ListNode) *ListNode {
	if pHead1 == nil {
		return pHead2
	}
	if pHead2 == nil {
		return pHead1
	}

	if pHead1.Val <= pHead2.Val {
		pHead1.Next = Merge(pHead1.Next, pHead2)
		return pHead1
	}
	if pHead1.Val > pHead2.Val {
		pHead2.Next = Merge(pHead1, pHead2.Next)
		return pHead2
	}
	return nil
}
```

时间复杂度：O(m+n)
空间复杂度：O(m+n),每一次递归，递归栈都会保存一个变量，最差情况会保存(m+n)个变量

**方法二（递归）：**

```go
/**
 * 合并两个排序的链表（递归）
 *
 * @param pHead1 ListNode类
 * @param pHead2 ListNode类
 * @return ListNode类
 */
func Merge(pHead1 *ListNode , pHead2 *ListNode) *ListNode {
	// 初始化
    ret := &ListNode{}
	current := ret
    
    // 遍历链表，比较大小
	for pHead1 != nil && pHead2 != nil {
		if pHead1.Val <= pHead2.Val {
			current.Next = pHead1
			pHead1 = pHead1.Next
		} else {
			current.Next = pHead2
			pHead2 = pHead2.Next
		}
		current = current.Next
	}
    
    // 尾节点
	if pHead1 != nil {
		current.Next = pHead1
	}
	if pHead2 != nil {
		current.Next = pHead2
	}
	return ret.Next
}
```

时间复杂度：O(m+n),m，n分别为两个单链表的长度
空间复杂度：O(1)

####  [JZ52-简单] 两个链表的第一个公共结点

##### 描述

输入两个无环的单链表，找出它们的第一个公共结点。（注意因为传入数据是链表，所以错误测试数据的提示是用其他方式显示的，保证传入数据是正确的）

##### 示例1

```
// 输入
{1,2,3},{4,5},{6,7}
// 返回值
{6,7}
```

说明：

```
第一个参数{1,2,3}代表是第一个链表非公共部分，第二个参数{4,5}代表是第二个链表非公共部分，最后的{6,7}表示的是2个链表的公共部分
这3个参数最后在后台会组装成为2个两个无环的单链表，且是有公共节点的     
```

##### 示例2

```
// 输入
{1},{2,3},{}
// 返回值
{}
```

说明：

```
2个链表没有公共节点 ,返回null，后台打印{}  
```

##### 解题思路

由题意可知，我们要找的公共节点就是如下图所示的 8

![image-20210728231444867](../../Image/image-20210728231444867.png)

可以利用双指针法，设 A 链表长度为 a，B链表长度为 b，则得：`a+b=b+a`，所以将`a+b`作为 A 链表的新长度，`b+a`作为 B 链表的新长度

##### 代码实现

```go
type ListNode struct{
    Val int
    Next *ListNode
}

/**
 * 两个链表的第一个公共节点
 *
 * @param pHead1 ListNode类
 * @param pHead2 ListNode类
 * @return ListNode类
 */
func FindFirstCommonNode(pHead1 *ListNode,  pHead2 *ListNode) *ListNode {
	p1, p2 := pHead1, pHead2
	for p1 != p2 {
		if p1 == nil {
			p1 = pHead2
		} else {
			p1 = p1.Next
		}
		if p2 == nil {
			p2 = pHead1
		} else {
			p2 = p2.Next
		}
	}
    // return p1 或 p2
	return p1
}
```

时间复杂度：O(m+n)
空间复杂度：O(1)

#### [JZ24-简单] 反转链表

##### 题目描述

输入一个链表，反转链表后，输出新链表的表头。

##### 示例

```
// 输入
{1,2,3}
// 返回值
{3,2,1}

// 输入
{}
// 返回值
{}
```

##### 代码实现

**双链表法：**

PHP

```php
<?php
/*class ListNode{
    var $val;
    var $next = NULL;
    function __construct($x){
        $this->val = $x;
    }
}*/
function reverseList($pHead)
{
    $head = null;
    while ($pHead) {
        $tmp = $pHead->next;
        $pHead->next = $head;
        $head = $pHead;
        $pHead = $tmp;
    }
    return $head;
}
```

Golang

```golang
/**
 * [JZ24-简单]反转链表
 *
 * @param head Node类
 * @return Node类
 */
func reverseList(head *Node) *Node {
	var newHead *Node
	var tmp *Node
	for head != nil {
		// 暂存下一个节点
		tmp = head.Next
		// 把新链表挂到原链表后面
		head.Next = newHead
		// 更新新链表
		newHead = head
		// 移动指针
		head = tmp
	}

	return newHead
}
```

**使用栈实现**：

```go
/**
 * [JZ24-简单] 反转链表-栈
 *
 * @param head ListNode类
 * @return ListNode类
 */
func reverseList2(head *ListNode) *ListNode {
	if head == nil {
		return head
	}
	tmpMap := make([]int, 0)
	for head != nil {
		tmpMap = append([]int{head.Val}, tmpMap...)
		head = head.Next
	}

	rHead := &ListNode{Val: tmpMap[0], Next: nil}
	cHead := rHead
	for i := 1; i < len(tmpMap); i++ {
		tmpHead := &ListNode{Val: tmpMap[i], Next: nil}
		cHead.Next = tmpHead
		cHead = cHead.Next
	}
	return rHead
}
```

#### [JZ18-简单] 删除链表的节点

##### 题目描述

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。返回删除后的链表的头节点。

1.此题对比原题有改动

2.题目保证链表中节点的值互不相同

3.该题只会输出返回的链表和结果做对比，所以若使用 C 或 C++ 语言，你不需要 free 或 delete 被删除的节点

数据范围:

0<=链表节点值<=10000

0<=链表长度<=10000

##### 示例1

```
// 输入
{2,5,1,9},5
// 返回值
{2,1,9}
// 说明
给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 2 -> 1 -> 9   
```

##### 示例2

```
// 输入
{2,5,1,9},1
// 返回值
{2,5,9}
// 说明
给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 2 -> 5 -> 9   
```

##### 解题思路

删除节点，需要把下一个节点的值移动到当前删除节点，然后更改当前节点的 Next

```
node.Val = node.Next.Val
node.Next = node.Next.Next
```

##### 代码实现

```go
/**
 * [JZ18]删除链表的节点
 *
 * @param head Node类
 * @param val int整型
 * @return Node类
 */
func deleteNode(head *Node, val int) *Node {
	if head.Value == val {
		return head.Next
	}
	pre := head
	for nil != head {
		if head.Value == val {
			head.Value = head.Next.Value
			head.Next = head.Next.Next
			break
		}
		head = head.Next
	}
	return pre
}

// 简化版 - 直接从 head 的 Next 节点开始遍历
func deleteNodeSimplify(head *Node, val int) *Node {
	if head.Value == val {
		return head.Next
	}
	pre := head
	for head.Next.Value != val {
		head = head.Next
	}
	head.Next = head.Next.Next
	return pre
}
```

#### [JZ76-中等] 删除链表中重复的结点

##### 题目描述

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表 1->2->3->3->4->4->5 处理后为 1->2->5

数据范围：链表长度满足 0 \le n \le 1000 \0≤*n*≤1000 ，链表中的值满足 1 \le val \le 1000 \1≤*v**a**l*≤1000 

进阶：空间复杂度 O(n)\*O*(*n*) ，时间复杂度 O(n) \*O*(*n*) 

例如输入{1,2,3,3,4,4,5}时，对应的输出为{1,2,5}

##### 示例

```
// 输入：
{1,2,3,3,4,4,5}
// 返回值：
{1,2,5}

// 输入：
{1,1,1,8}
// 返回值：
{8}
```

##### 代码实现

```go
/**
 * [JZ76-简单]删除链表中重复的结点
 *
 * @param pHead Node类
 * @return Node类
 */
func deleteDuplication(pHead *Node) *Node {
	// 定义一个空链表
	var res = new(Node)
	// 给新链表添加表头
	res.Next = pHead
	cur := res
	// 从 next 节点开始遍历（前面加了一个表头）
	for nil != cur.Next && nil != cur.Next.Next {
		if cur.Next.Value == cur.Next.Next.Value {
			// 遇到前后节点相同情况，进行遍历，跳过所有相同节点
			tmp := cur.Next.Value
			for nil != cur.Next && cur.Next.Value == tmp {
				cur.Next = cur.Next.Next
			}
		} else {
			// 循环链表
			cur = cur.Next
		}
	}
	return res.Next
}
```

#### [JZ23-中等] 链表中环的入口结点

##### 题目描述

给一个长度为 n 的链表，若其中包含环，请找出该链表的环的入口结点，否则，返回 null。

数据范围： n\le10000*n*≤10000，1<=结点值<=100001<=结点值<=10000

要求：空间复杂度 O(1)*O*(1)，时间复杂度 O(n)*O*(*n*)

例如，输入{1,2},{3,4,5}时，对应的环形链表如下图所示：

<img src="../../Image/image-202205082211.png" alt="image-202205082211" style="zoom:50%;" />

可以看到环的入口结点的结点值为3，所以返回结点值为3的结点。

- 输入描述：输入分为 2 段，第一段是入环前的链表部分，第二段是链表环的部分，后台会根据第二段是否为空将这两段组装成一个无环或者有环单链表

- 返回值描述：返回链表的环的入口结点即可，我们后台程序会打印这个结点对应的结点值；若没有，则返回对应编程语言的空结点即可。

##### 示例

```
// 输入
{1,2},{3,4,5}
// 返回值
// 返回环形链表入口结点，我们后台程序会打印该环形链表入口结点对应的结点值，即 3
3

// 输入
{1},{}
// 返回值
// 没有环，返回对应编程语言的空结点，后台程序会打印"null"
"null"

// 输入
{},{2}
// 返回值
// 环的部分只有一个结点，所以返回该环形链表入口结点，后台程序打印该结点对应的结点值，即 2
2
```

##### 解题思路

快慢指针法：

快慢指针可以很容易判断一条链表是否存在环，快指针 fast 每次走两步，慢指针 slow 每次走一步，那么若进入环中，每次他们之间的相对距离都会 -1，直到两者相遇。

那么，怎么使用快慢指针找到环的入口呢？

<img src="../../Image/image-202205082311.png" alt="image-202205082311" style="zoom:50%;" >

在慢指针 slow 进入链表环之前，快指针 fast 已经进入了环，且在里面循环，这才能在慢指针进入环之后，快指针追到了慢指针（第一次相遇）

假设：fast 指针在环中走了 n 圈，slow 指针在环中走了 m 圈时，两指针相遇。环前距离为 x，入口节点到第一次相遇节点相差距离为 y，相遇节点到环入口为距离为 z。则有：

- x + n * (y + z) + y（fast 指针第一次相遇步数）
- x + m * (y + z) + y（slow 指针第一次相遇步数）
- 则：x + n * (y + z) + y = 2 * (x + m * (y + z) + y)
- 得：x + y = (n − 2m)(y + z) 
- 即：从链表头经过环入口到达相遇地方经过的距离等于整数倍环的大小
- 那如果 fast 指针从头开始遍历到相遇位置，slow 指针从相遇位置开始在环中遍历，使用相同的步数，最后会在 y 节点处相遇，由于步数相同，所以在环入口处已经相遇，y处属于重复相遇。

示例：

```
输入：{1,2},{3,4,5}
fast 每次走 2 步，slow 每次走 1 步，第一次相遇
fast：3 -> 5 -> 4
slow：2 -> 3 -> 4

令快指针 fast 为 head，这时快慢指针同时一步一步走
fast：2 -> 3
slow：5 -> 3
再次相遇节点 3 即为入口节点
```

因此，可以得出结论：

> 快指针 fast 每次走两步，慢指针 slow 每次走一步，第一次相遇后，令快指针 fast 为 head，这时快慢指针同时一步一步走，再次相遇点即为环的入口

##### 代码实现

```go
/**
 * [JZ23-中等]链表中环的入口结点
 *
 * @param pHead Node类
 * @return Node类
 */
func entryNodeOfLoop(pHead *Node) *Node {
	if nil == pHead || nil == pHead.Next {
		return nil
	}
	fast, slow := pHead, pHead
	for nil != fast && nil != fast.Next.Next {
		fast = fast.Next.Next
		slow = slow.Next
		if fast == slow {
			fast = pHead
			for fast != slow {
				fast = fast.Next
				slow = slow.Next
			}
			return fast
		}
	}
	return nil
}
```

#### [JZ35-较难] 复杂链表的复制

##### 题目描述

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针random指向一个随机节点），请对此链表进行深拷贝，并返回拷贝后的头结点。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）。 下图是一个含有5个结点的复杂链表。图中实线箭头表示next指针，虚线箭头表示random指针。为简单起见，指向null的指针没有画出。

<img src="../../Image/image-202205152117.png" alt="image-202205152117" style="zoom:50%;" >

##### 示例

输入：{1,2,3,4,5,3,5,#,2,#}

输出：{1,2,3,4,5,3,5,#,2,#}

解析：我们将链表分为两段，前半部分{1,2,3,4,5}为ListNode，后半部分{3,5,#,2,#}是随机指针域表示。

以上示例前半部分可以表示链表为的ListNode:1->2->3->4->5

后半部分，3，5，#，2，#分别的表示为

1的位置指向3，2的位置指向5，3的位置指向null，4的位置指向2，5的位置指向null

如下图：

![image-202205211900](../../Image/image-202205211900.png)

##### 代码实现

```go

```

### 树

#### [JZ55-简单] 二叉树的深度

##### 题目描述

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

##### 示例

```
// 输入
{1,2,3,4,5,#,6,#,#,7}
// 返回值：
4
```

##### 代码实现

PHP

```php
<?php

/*class TreeNode{
    var $val;
    var $left = NULL;
    var $right = NULL;
    function __construct($val){
        $this->val = $val;
    }
}*/
function TreeDepth($pRoot)
{
    if($pRoot == null) return 0;
    $left = TreeDepth($pRoot->left);
    $right = TreeDepth($pRoot->right);
    return $left > $right ? $left + 1 : $right +1;
}
```

Golang

```go
type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}

/**
 * 二叉树的深度
 * 
 * @param pRoot TreeNode 类 
 * @return int 整型
*/
func TreeDepth( pRoot *TreeNode ) int {
    if (pRoot == nil) {
        return 0
    }

    left,right := 0,0
    left = TreeDepth(pRoot.Left)
    right = TreeDepth(pRoot.Right)
    
    depth := left
    if (right > left) {
        depth = right
    }
    return depth + 1
}
```

#### [JZ27-简单] 二叉树的镜像

##### 题目描述

操作给定的二叉树，将其变换为源二叉树的镜像。

```
比如：    源二叉树 
            8
           /  \
          6   10
         / \  / \
        5  7 9 11
        镜像二叉树
            8
           /  \
          10   6
         / \  / \
        11 9 7  5
```

##### 示例

```
// 输入
{8,6,10,5,7,9,11}
// 返回
{8,10,6,11,9,7,5}
```

##### 代码实现

PHP

```php
<?php
/*class TreeNode{
    var $val;
    var $left = NULL;
    var $right = NULL;
    function __construct($val){
        $this->val = $val;
    }
}*/

function Mirror(&$root)
{
    if(!$root)return;
    $temp1 = $temp2 = NULL;
    if($root->left){
        $temp1 = Mirror($root->left);
    }
    if($root->right){
        $temp2 = Mirror($root->right);
    }
    $root->left = $temp2;
    $root->right = $temp1;
    return $root;
}
```

Golang

```go
/**
 * 二叉树的镜像
 *
 * @param pRoot TreeNode 类 
 * @return TreeNode 类
*/
func Mirror(pRoot *TreeNode) *TreeNode {
    if nil == pRoot {
        return pRoot
    }
    pRoot.Left, pRoot.Right = Mirror(pRoot.Right), Mirror(pRoot.Left)
    return pRoot
}
```

#### [JZ32-简单] 从上往下打印二叉树

##### 题目描述

不分行从上往下打印出二叉树的每个节点，同层节点从左至右打印。例如输入{8,6,10,#,#,2,1}，如以下图中的示例二叉树，则依次打印8,6,10,2,1(空节点不打印，跳过)，请你将打印的结果存放到一个数组里面，返回。

```
    8
   /  \
  6   10
      / \
     2   1
```

数据范围:

0<=节点总数<=1000

-1000<=节点值<=1000

##### 示例

```
// 输入：
{8,6,10,#,#,2,1}
// 返回值：
[8,6,10,2,1]

// 输入：
{5,4,#,3,#,2,#,1}
// 返回值：
[5,4,3,2,1]
```

##### 解题思路

层次遍历

##### 代码实现

```go
/**
 * [JZ32-简单]从上往下打印二叉树
 * 
 * @param root TreeNode类 
 * @return int 整型一维数组
*/
func printFromTopToBottom(root *TreeNode) []int {
    res := []int{}
    if nil == root {
        return res
    }
    
    tmp := []*TreeNode{root}
    for i := 0; i < len(tmp); i++ {
        current := tmp[i]
        res = append(res, current.Val)
        if nil != current.Left {
            tmp = append(tmp, current.Left)
        }
        if nil != current.Right {
            tmp = append(tmp, current.Right)
        }
    }
    return res
}
```

#### [JZ68-简单] 二叉搜索树的最近公共祖先

##### 题目描述

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

1.对于该题的最近的公共祖先定义:对于有根树T的两个节点p、q，最近公共祖先LCA(T,p,q)表示一个节点x，满足x是p和q的祖先且x的深度尽可能大。在这里，一个节点也可以是它自己的祖先.

2.二叉搜索树是若它的左子树不空，则左子树上所有节点的值均小于它的根节点的值； 若它的右子树不空，则右子树上所有节点的值均大于它的根节点的值

3.所有节点的值都是唯一的。

4.p、q 为不同节点且均存在于给定的二叉搜索树中。

数据范围:

3<=节点总数<=10000

0<=节点值<=10000

如果给定以下搜索二叉树: {7,1,12,0,4,11,14,#,#,3,5}，如下:

```
 	     7
 	   /   \
 	1	     12
   /  \     /  \
  0    4   11   14
      / \
     3   5
```

##### 示例

```
// 输入：
{7,1,12,0,4,11,14,#,#,3,5},1,12
// 返回值：
7
// 说明：节点1 和 节点12的最近公共祖先是7 

// 输入：
{7,1,12,0,4,11,14,#,#,3,5},12,11
// 返回值：
12
// 说明：因为一个节点也可以是它自己的祖先.所以输出12
```

##### 解题思路

二叉搜索树每个节点值大于它的左子节点，且大于全部左子树的节点值，小于它右子节点，且小于全部右子树的节点值。

递归思路：

对于某一个节点若是p与q都小于等于这个这个节点值，说明p、q都在这个节点的左子树，而最近的公共祖先也一定在这个节点的左子树；若是p与q都大于等于这个节点，说明p、q都在这个节点的右子树，而最近的公共祖先也一定在这个节点的右子树。而若是对于某个节点，p与q的值一个大于等于节点值，一个小于等于节点值，说明它们分布在该节点的两边，而这个节点就是最近的公共祖先，因此从上到下的其他祖先都将这个两个节点放到同一子树，只有最近公共祖先会将它们放入不同的子树，每次进入一个子树又回到刚刚的问题，因此可以使用递归。

##### 代码实现

```go
/**
 * [JZ68-简单]二叉搜索树的最近公共祖先
 *
 * @param root TreeNode类 
 * @param p int整型 
 * @param q int整型 
 * @return int整型
*/
func lowestCommonAncestor(root *TreeNode, p int, q int) int {
    if nil == root {
        return 0
    }
    
    // p q 都小于等于这个节点值，则 p q 都在左子树
    if root.Val > p && root.Val > q {
        return lowestCommonAncestor(root.Left, p, q)
    }

    // p q 都大于等于这个节点值，则 p q 都在右子树
    if root.Val < p && root.Val < q {
        return lowestCommonAncestor(root.Right, p, q)
    }

    return root.Val
}
```

#### [JZ79 -简单] 判断是不是平衡二叉树

##### 题目描述

输入一棵节点数为 n 二叉树，判断该二叉树是否是平衡二叉树。

在这里，我们只需要考虑其平衡性，不需要考虑其是不是排序二叉树

**平衡二叉树**（Balanced Binary Tree），具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

样例解释：

```
 		 1
 	   /   \
 	2	     3
   /  \     /  \
  4    5   6    7
```

样例二叉树如图，为一颗平衡二叉树

注：我们约定空树是平衡二叉树。

数据范围：n ≤ 100,树上节点的val值满足 0 10000 ≤ n ≤ 1000

要求：空间复杂度O(1)，时间复杂度 O(n)

- 输入描述：输入一棵二叉树的根节点
- 返回值描述：输出一个布尔类型的值

##### 示例

```
// 输入：
{1,2,3,4,5,6,7}
// 返回值：
true

// 输入：
{}
// 返回值：
true
```

##### 解题思路

通过判断左右子树深度差（高度差）不大于1

因此，分别求解左右子树高度差即可

##### 代码实现

```go
/**
 * [JZ79-简单] 判断是否是平衡二叉树
 *
 * @param pRoot TreeNode类 
 * @return bool布尔型
*/
func isBalancedBinaryTree(pRoot *TreeNode) bool {
    return treeDepth(pRoot) > -1
}

func treeDepth(root *TreeNode) int {
	if nil == root {
		return 0
	}

	leftLength := treeDepth(root.Left);
	rightLength := treeDepth(root.Right);

	if -1 == leftLength || -1 == rightLength || 1 < abs(leftLength - rightLength) {
		return -1
	}

	return max(leftLength, rightLength) + 1
}

func abs(a int) int {
    if 0 > a {
        return -a
    }
    return a
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

#### [JZ28 -简单] 对称的二叉树

##### 描述

给定一棵二叉树，判断其是否是自身的镜像（即：是否对称）
例如：下面这棵二叉树是对称的

```
 		 1
 	   /   \
 	2	     2
   /  \     /  \
  3    4   4    3
```

下面这棵二叉树不对称的：

```
 		 1
 	   /   \
 	  2	    2
       \     \
        3     3
```

数据范围：节点数满足 0≤*n*≤1000，节点上的值满足∣*v**a**l*∣≤1000

要求：空间复杂度 O(n)，时间复杂度 O(n)

##### 示例

```
// 输入：
{1,2,2,3,4,4,3}
// 返回值：
true

// 输入：
{8,6,9,5,7,7,5}
// 返回值
false
```

##### 解题思路

递归

##### 代码实现

```go
func recursion(root1 *TreeNode, root2 *TreeNode) bool {
	if nil == root1 && nil == root2 {
		return true
	}
	if nil == root1 || nil == root2 || root1.Val != root2.Val {
		return false
	}
	return recursion(root1.Left, root2.Right) && recursion(root1.Right, root2.Left)
}

/**
 * [JZ28-简单] 对称二叉树
 *
 * @param pRoot TreeNode 类
 * @return bool 布尔型
*/
func isSymmetricalTree(pRoot *TreeNode) bool {
    return recursion(pRoot, pRoot)
}
```

#### [JZ82-简单] 二叉树中和为某一值的路径(一)

##### 描述

给定一个二叉树 root 和一个值 sum ，判断是否有从根节点到叶子节点的节点值之和等于 sum 的路径。

1.该题路径定义为从树的根结点开始往下一直到叶子结点所经过的结点

2.叶子节点是指没有子节点的节点

3.路径只能从父节点到子节点，不能从子节点到父节点

4.总节点数目为n
例如：
给出如下的二叉树sum=22，

```
		 5
 	   /   \
 	  4	    8
    /  \     \
   1   11     9
  	  /  \  
  	  2   7
```

返回 true，因为存在一条路径 5→4→11→2 的节点值之和为 22

数据范围：

1.树上的节点数满足 0≤*n*≤10000

2.每 个节点的值都满足 |val|≤1000

要求：空间复杂度 O(n)，时间复杂度 O(n)

进阶：空间复杂度 O(树的高度)，时间复杂度 O(n)

##### 示例

```
// 输入：
{5,4,8,1,11,#,9,#,#,2,7},22
// 返回值：
true

// 输入：
{1,2},0
// 返回值：
false

// 输入：
{1,2},3
// 返回值：
true

// 输入：
{},0
// 返回值：
false
```

##### 解题思路

前序遍历

##### 代码实现

```go
/**
  * [JZ82-简单] 二叉树中和为某一值的路径(一)
  *
  * @param root TreeNode 类 
  * @param sum int 整型 
  * @return bool 布尔型
*/
func hasTreePathSum(root *TreeNode, sum int) bool {
    if nil == root {
		return false
	}
	if nil == root.Left && nil == root.Right && root.Val == sum {
		return true
	}

	return hasTreePathSum(root.Left, sum - root.Val) || hasTreePathSum(root.Right, sum - root.Val)
}
```

#### [JZ34-中等] 二叉树中和为某一值的路径(二)

##### 描述

输入一颗二叉树的根节点 root 和一个整数 expectNumber，找出二叉树中结点值的和为 expectNumber 的所有路径。

1.该题路径定义为从树的根结点开始往下一直到叶子结点所经过的结点

2.叶子节点是指没有子节点的节点

3.路径只能从父节点到子节点，不能从子节点到父节点

4.总节点数目为 n

如二叉树 root 为 {10,5,12,4,7} ，expectNumber 为 22

```
		 10
 	   /    \
 	  5	     12
    /  \     
   4    7     
```

则合法路径有[[10,5,7],[10,12]]

数据范围:

树中节点总数在范围 [0, 5000] 内

-1000 <= 节点值 <= 1000

-1000 <= expectNumber <= 1000

##### 示例

```
// 输入：
{10,5,12,4,7},22
// 返回值：
[[10,5,7],[10,12]]（返回[[10,12],[10,5,7]]也是对的）

// 输入：
{10,5,12,4,7},15
// 返回值：
[]

// 输入：
{2,3},0
// 返回值：
[]

// 输入：
{1,3,4},7
// 返回值：
[]
```

##### 解题思路

深度优先搜索

- 维护两个向量`ret`和`path`
- 编写递归函数`dfs`
- 递归函数内部要处理更新`path`，更新`expectNumber`，判断是否为叶子节点和判断是否要将`path`追加到`ret`末尾

##### 代码示例

```go
var ret [][]int

func dfs(root *TreeNode, expectNumber int, path []int) {
	if nil == root {
		return
	}
	if nil == root.Left && nil == root.Right && root.Val == expectNumber {
		ret = append(ret, append(path, root.Val))
		return
	}
	dfs(root.Left, expectNumber - root.Val, append(path, root.Val))
	dfs(root.Right, expectNumber - root.Val, append(path, root.Val))
}

/**
  * [JZ34-简单] 二叉树中和为某一值的路径(二)
  *
  * @param root TreeNode 类 
  * @param sum int 整型 
  * @return bool 布尔型
*/
func findTreePathSum(root *TreeNode, expectNumber int) [][]int {
	dfs(root, expectNumber, []int{})
	return ret
}
```

#### [中等]按之字形顺序打印二叉树

##### 描述

给定一个二叉树，返回该二叉树的之字形层序遍历，（第一层从左向右，下一层从右向左，一直这样交替）

数据范围：![img](https://www.nowcoder.com/equation?tex=0%20%5Cle%20n%20%5Cle%201500%20%5C),树上每个节点的 val 满足 ![img](https://www.nowcoder.com/equation?tex=%7Cval%7C%20%3C%3D%20100%20%5C)
要求：空间复杂度：![img](https://www.nowcoder.com/equation?tex=O(n)%20%20%5C)，时间复杂度：![img](https://www.nowcoder.com/equation?tex=O(n)%20%5C)

例如：
给定的二叉树是{1,2,3,#,#,4,5}

![img](../../Image/image-202110092115.png)

该二叉树之字形层序遍历的结果是

```
[
[1],
[3,2],
[4,5]
]
```

##### 示例

```
// 输入1
{1,2,3,#,#,4,5}
// 返回值1
[[1],[3,2],[4,5]]

// 输入2
{8,6,10,5,7,9,11}
// 返回值2
[[8],[10,6],[5,7,9,11]]

// 输入3
{1,2,3,4,5}
// 返回值3
[[1],[3,2],[4,5]]
```

##### 代码实现

```go

```

### 队列 & 栈

#### [JZ9-简单] 用两个栈实现队列

##### 题目描述

用两个栈来实现一个队列，使用n个元素来完成 n 次在队列尾部插入整数(push)和n次在队列头部删除整数(pop)的功能。 队列中的元素为int类型。保证操作合法，即保证pop操作时队列内已有元素。

数据范围： n≤1000

要求：存储n个元素的空间复杂度为 O(n) ，插入与删除的时间复杂度都是 O(1)

##### 示例1

输入：

```
// 输入
["PSH1","PSH2","POP","POP"]

// 返回值
1,2

// 说明
"PSH1":代表将1插入队列尾部
"PSH2":代表将2插入队列尾部
"POP“:代表删除一个元素，先进先出=>返回1
"POP“:代表删除一个元素，先进先出=>返回2    
```

##### 示例2

```
// 输入
["PSH2","POP","PSH1","POP"]

// 返回值
2,1
```

##### 代码实现

PHP：

```php
<?php

$queue = array();
function mypush($node)
{
    global $queue;
    return array_push($queue,$node);
}
function mypop()
{
    global $queue;
    return array_shift($queue);
}
```

Go：

```go
var stack1 [] int
var stack2 [] int

func Push(node int) {
    stack1 = append(stack1, node)
}

func Pop() int{
    var result int
    // stack2 为空，将 stack1 中的元素逐个入栈 stack2，再弹出 stack2 栈顶元素
    if len(stack2) == 0 {
        for i := len(stack1) - 1; i > 0; i-- {
            stack2 = append(stack2, stack1[i])
        }
        result = stack1[0]
        stack1 = []int{}
    } else {// 弹出 stack2 栈顶元素
        result = stack2[len(stack2)-1]
        stack2 = stack2[:len(stack2)-1]
    }
    return result
}
```

#### [JZ30-简单] 包含min函数的栈

##### 描述

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的 min 函数，输入操作时保证 pop、top 和 min 函数操作时，栈中一定有元素。

此栈包含的方法有：

- push(value):将value压入栈中
- pop():弹出栈顶元素
- top():获取栈顶元素
- min():获取栈中最小元素

数据范围：操作数量满足 0≤*n*≤300 ，输入的元素满足 |val|≤10000 
进阶：栈的各个操作的时间复杂度是 O(1)，空间复杂度是 O(n)

##### 示例

```
 // 输入：
 ["PSH-1","PSH2","MIN","TOP","POP","PSH1","TOP","MIN"]
 // 输出：
 -1,2,1,-1
```

"PSH-1"表示将-1压入栈中，栈中元素为-1

"PSH2"表示将2压入栈中，栈中元素为2，-1

“MIN”表示获取此时栈中最小元素==>返回-1

"TOP"表示获取栈顶元素==>返回2

"POP"表示弹出栈顶元素，弹出2，栈中元素为-1

"PSH1"表示将1压入栈中，栈中元素为1，-1

"TOP"表示获取栈顶元素==>返回1

“MIN”表示获取此时栈中最小元素==>返回-1

##### 代码实现

```go
var stack []int

func Push(node int) {
    stack = append(stack, node)
}

func Pop() {
    stack = stack[0:len(stack)-1]
}

func Top() int {
    result := stack[len(stack) - 1]
    return result
}

func Min() int {
    min := stack[0]
    for _, v := range stack {
        if min > v {
            min = v
        }
    }
    return min
}
```

#### [JZ73-简单] 翻转单词序列

##### 题目描述

最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“nowcoder. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a nowcoder.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

数据范围：1≤*n*≤100 
进阶：空间复杂度 O(n)，时间复杂度 O(n)，保证没有只包含空格的字符串

##### 示例

```
// 输入：
"nowcoder. a am I"
// 输出：
"I am a nowcoder."

// 输入：
""
// 输出：
""
```

##### 解题思路

- 遍历字符串，将整个字符串按照空格分割然后入栈。
- 遍历栈，将栈中内容弹出拼接成字符串。

##### 代码实现

```go
/**
 * [JZ73-简单] 翻转单词序列
 *
 * 
 * @param str string 字符串 
 * @return string 字符串
*/
func reverseSentence(str string) string {
    strArr := strings.Split(str, " ")
    result := ""
    for i := len(strArr) - 1; i >= 0; i-- {
        result += strArr[i]
        if i > 0 {
            result += " "
        }
    }
    return result
}
```

## 算法

### 搜索算法

#### [JZ11-简单] 旋转数组的最小数字

##### 描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序数组的一个旋转，输出旋转数组的最小元素。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

##### 示例

输入：

```
[3,4,5,1,2]
```

返回值：

```
1
```

##### 解题思路

由题意，非递减排序的数组即为递增数组，所以本题输入是一个递增数组的旋转

第一种做法比较简单，可以使用暴力法，更优的解法是二分法

##### 代码实现

二分 - PHP

```php
function minNumberInRotateArray($rotateArray)
{
	$count = count($rotateArray);
    if($count == 0) return 0;
    if ($count == 1) return $rotateArray[0];
    $left = 0;
    $right = count($rotateArray) - 1;
    while ($rotateArray[$left] >= $rotateArray[$right]) {
    	if ($right - $left == 1) {
    		$mid = $right;
    		break;
    	}
    	$mid = floor(($left + $right) / 2);
    	if ($rotateArray[$mid] > $rotateArray[$right]) {
    		$left = $mid;
    	} elseif ($rotateArray[$mid] == $rotateArray[$right]) {
    		--$right;
    	} else {
    		$right = $mid;
    	}
    }
    return $rotateArray[$mid];
}
```

二分 - Golang

```go
/**
 * 旋转数组的最小数字
 *
 * @param rotateArray int整型一维数组
 * @return int整型
 */
func minNumberInRotateArray(rotateArray []int) int {
	// 边界值处理
	if len(rotateArray) == 0 {
		return 0
	}
	if len(rotateArray) == 1 {
		return rotateArray[0]
	}

	// 初始化中间变量
	var mid int
	// 定义左右变量
	left, right := 0, len(rotateArray) - 1
	for rotateArray[left] >= rotateArray[right] {
		// 退出条件
		if (right - left) == 1 {
			mid = right
			break
		}
		// 中间变量
		mid = (left + right) / 2
		if rotateArray[mid] > rotateArray[right] {// 中间值大于最右侧值，说明最小值在右侧
			left = mid
		} else if rotateArray[mid] < rotateArray[right] {// 中间值小于最右侧值，说明最小值在左侧
			right = mid
		} else {
			right--
		}
	}
	return rotateArray[mid]
}
```

#### [JZ53-简单] 数字在升序数组中出现的次数

##### 描述

给定一个长度为 n 的非降序数组和一个非负数整数 k ，要求统计 k 在数组中出现的次数

数据范围：1000≤*n*≤1000,0≤*k*≤100，数组中每个元素的值满足 0≤val≤100
要求：空间复杂度 O(1)，时间复杂度 O(logn)

##### 示例

```
// 输入
[1,2,3,3,3,3,4,5],3
// 输出
4

// 输入
[1,3,4,5],6
// 输出
0
```

##### 解题思路

由题意，非降序数组，即升序数组，因此，可以使用二分法

- 先通过二分法找出 k 在数组中的位置
- 再分别向左右查找

##### 代码实现

```go
/**
 * [JZ53-简单] 数字在升序数组中出现的次数
 *
 * @param data int 整型一维数组
 * @param k int 整型
 * @return int 整型
 */
func getNumFromAscArray(data []int, k int) int {
	if 0 == len(data) {
		return 0
	}

	// 二分查找 k 位置
	left, right, mid, isExist := 0, len(data)-1, 0, false
	for left <= right {
		mid = (left + right) / 2
		if data[mid] == k {
			isExist = true
			break
		} else if data[mid] > k {
			right = right - 1
		} else {
			left = left + 1
		}
	}

	if false == isExist {
		return 0
	}

	// // 向左遍历查找
	count, move := 1, mid
	for {
		move--
		if move < 0 || data[move] != k {
			break
		}
		count++
	}

	// 向右遍历查找
	move = mid
	for {
		move++
		if move >= len(data) || data[move] != k {
			break
		}
		count++
	}

	return count
}
```

#### [JZ44-简单] 数字序列中某一位的数字

##### 描述

数字以 0123456789101112131415... 的格式作为一个字符序列，在这个序列中第 2 位（**从下标 0 开始计算**）是 2 ，第 10 位是 1 ，第 13 位是 1 ，以此类题，请你输出第 n 位对应的数字。

数据范围： 0≤*n*≤10^9 

##### 示例

```
// 输入
0
// 输出
0

// 输入
2
// 输出
2

// 输入
10
// 输出
1

// 输入
13
// 输出
1
```

##### 解题思路

```
// 区间规律
0
1 ~ 9：1 * 9 = 9
10 ~ 99：2 * 90 = 180
100 ~ 999：3 * 900 = 2700
...

// n 属于哪个自然数
// 0 1 2 3 4 5 6 7 8 9 10 11 12 13
n   - 区间首位 - 位数 - 所属自然数 - 下标
2   -   1    -  1  -  2=1+(2-1)/1      -   0=(2-1)%1
10  -   10   -  2  -  10=10+(10-9-1)/2  -   0=(10-9-1)%2
13  -   10   -  2  -  11=10+(13-9-1)/2  -   1=(13-9-1)%2

所属自然数 = 区间首位数 + 剩余部分除以这个区间的位数
```

从以上示例可以发现 n-1 和区间之间的规律

```
位数：digit
区间首位：start
区间总位数：sum = digit * 9 * start
区间剩余部分：x
所属自然数：start + (x-1)/digit
```

- 确定 n 属于哪个区间
  - 通过 n 不断减去前面区间的位数
- 确定 n 属于哪个自然数
  -  区间首位数 + 剩余部分 / 这个区间的位数
- 确定 n 在自然数中的位置
  - n - 1 对这个位数取模

##### 代码实现

```go
/**
 * [JZ44-简单] 数字序列中某一位的数字
 *
 * @param data int 整型一维数组
 * @param k int 整型
 * @return int 整型
 */
func findNthDigit(n int) int {
	// 位数、区间首位数字、区间总位数
	digit, start, sum := 1, 1, 9

	// 获取位数、区间首位、区间总位数
	for n > sum {
		n -= sum
		start *= 10
		digit += 1
		sum = 9 * start * digit
	}

	// 定位 n 在哪个数上
	num := start + (n-1)/digit

	// 定位 n 在这个数的哪一位上
	index := (n - 1) % digit

	// 将 num 转化成 string
	numStr := strconv.Itoa(num)

	// 根据 index 索引获取字符串值
	numStr = string(numStr[index])

	// 将字符串转化成 int
	ret, _ := strconv.Atoi(numStr)

	return ret
}
```

#### [JZ4-中等]二维数组中的查找

##### 题目描述

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

##### 代码实现

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

### 动态规划

#### [入门] 斐波那契数列

##### 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0，第1项是1）。

n<=39

##### 解题思路

斐波那契数列指的是这样一个数列：0、1、1、2、3、5、8、13、21、34、……在数学上，斐波那契数列以如下被以递推的方法定义：*F*(0)=0，*F*(1)=1, *F*(n)=*F*(n - 1)+*F*(n - 2)（*n* ≥ 3，*n* ∈ N*）

##### 代码实现

动规 - PHP

```php
function Fibonacci($n)
{
    if ($n == 0 || $n == 1) return $n;
    $pre = 0;
    $next = 1;
    $res = 0;
    for ($i = 2; $i <= $n; $i++) {
        $res = $pre + $next;
        $pre = $next;
        $next = $res;
    }
    return $res;
}
```

递归法 - PHP

```php
<?php
function Fibonacci($n)
{
    if ($n == 0 || $n == 1) return $n;
    return Fibonacci($n - 1) + Fibonacci($n + 1);
}
```

递归法 - Golang

```go
/**
 * 递归
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

#### [简单] 跳台阶

##### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）

##### 解题思路

```
n -- m    n -- m
1 -- 1    5 -- 8(11111、122、212、221、1112、1211、1121、2111)
2 -- 2
3 -- 3    m = (n-1) + (n-2)
4 -- 5
```

由题意可知此题和斐波那契数列解法相同

##### 代码实现

动规 - PHP

```php
function jumpFloor($number)
{
    if ($number === 1 || $number === 2) return $number;
    $pre = 1;
    $next = 2;
    for ($i = 3; $i <= $number; $i++) {
    	$sum = $pre + $next;
    	$pre = $next;
    	$next = $sum;
    }
    return $sum;
}
```

递归 - PHP

```php
function jumpFloor($number)
{
    if ($number === 1 || $number === 2) return $number;
    $sum = jumpFloor($number - 1) + jumpFloor($number - 2);
    return $sum;
}
```

递归 - GOlang

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

#### [简单] 跳台阶扩展问题

##### 描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶(n为正整数)总共有多少种跳法。

##### 示例

```
输入：3
输出：4
```

##### 解题思路

```
n -- m    n -- m
1 -- 1    4 -- 8    m = n^(2-1)
2 -- 2    5 -- 16
3 -- 3
```

由题意可知结果为2的n-1次方，所以可以转化为一个数学问题

##### 代码实现

PHP

```php
<?php

function jumpFloorII($number)
{
    if($number == 1) return 1;
    return pow(2,($number - 1));
}
```

Golang

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

#### [JZ42-简单] 连续子数组的最大和

##### 描述

输入一个长度为 n 的整型数组 array，数组中的一个或连续多个整数组成一个子数组，子数组最小长度为1。求所有子数组的和的最大值。

数据范围:

- 1<=*n*<=2×105
- −100<=*a*[*i*]<=100

要求：时间复杂度为 O(n)，空间复杂度为 O(n)

进阶：时间复杂度为 O(n)，空间复杂度为 O(1)

##### 示例

```
// 输入
[1,-2,3,10,-4,7,2,-5]
// 输出：输入数组的子数组[3,10,-4,7,2]可以求得最大和为18
18

// 输入
[2]
// 输出
2

// 输入
[-10]
// 输出
10
```

##### 解题思路

动态规划

##### 代码实现

```go
/**
 * [JZ44-简单] 数字序列中某一位的数字
 *
 * @param array int 一维数组
 * @return int
 */
func findGreatestSumOfSubArray(array []int) int {
	length := len(array)
	if 1 == length {
		return array[0]
	}

	dp := make([]int, length)
	dp[0] = array[0]
	max := array[0]
	for i := 1; i < length; i++ {
		dp[i] = int(math.Max(float64(dp[i-1]+array[i]), float64(array[i])))
		max = int(math.Max(float64(dp[i]), float64(max)))
	}
	return max
}
```

#### [JZ85-中等] 连续子数组的最大和(二)

##### 描述

输入一个长度为n的整型数组array，数组中的一个或连续多个整数组成一个子数组，找到一个具有最大和的连续子数组。

- 子数组是连续的，比如[1,3,5,7,9]的子数组有[1,3]，[3,5,7]等等，但是[1,3,7]不是子数组
- 如果存在多个最大和的连续子数组，那么返回其中长度最长的，该题数据保证这个最长的只存在一个
- 该题定义的子数组的最小长度为1，不存在为空的子数组，即不存在[]是某个数组的子数组
- 返回的数组不计入空间复杂度计算

数据范围:

- 1<=n<=10^5
- -100 <= a[i] <= 100

要求:时间复杂度O(n)，空间复杂度O(n)

进阶:时间复杂度O(n)，空间复杂度O(1)

##### 示例

```
// 输入：
[1,-2,3,10,-4,7,2,-5]
// 返回值：
[3,10,-4,7,2]
// 说明：经分析可知，输入数组的子数组[3,10,-4,7,2]可以求得最大和为18，故返回[3,10,-4,7,2]

// 输入：
[1,2,-3,4,-1,1,-3,2]
// 返回值：
[1,2,-3,4,-1,1]
// 说明：经分析可知，最大子数组的和为4，有[4],[4,-1,1],[1,2,-3,4],[1,2,-3,4,-1,1]，故返回其中长度最长的[1,2,-3,4,-1,1]

// 输入：
[-2,-1]
// 返回值：
[-1]
// 说明：子数组最小长度为1，故返回[-1] 
```

##### 解题思路

动态规划+滑动区间

##### 代码实现

```go
package main

import "fmt"

/**
 * [JZ42-简单] 连续子数组的最大和
 *
 * @param array int 一维数组
 * @return int
 */
func findGreatestSumOfSubArr(array []int) []int {
	length := len(array)
	if 1 == length {
		return array
	}

	dp := make([]int, length)
	dp[0] = array[0]
	max := array[0]

	// 滑动左右区间值 left、right 和最长区间值 l、r
	left, right, maxLeft, maxRight := 0, 0, 0, 0
	for i := 1; i < length; i++ {
		right++

		// 前面连续数字和小于当前数，则重新开始
		if dp[i-1]+array[i] < array[i] {
			dp[i] = array[i]
			left = right
		} else {
			dp[i] = dp[i-1] + array[i]
		}

		if dp[i] > max || dp[i] == max && (right-left) > (maxRight-maxLeft) {
			max = dp[i]
			maxLeft = left
			maxRight = right
		}
	}

	return array[maxLeft : maxRight+1]
}

func main() {
	arr := []int{1, -2, 3, 10, -4, 7, 2, -5}
	ret := findGreatestSumOfSubArr(arr)
	fmt.Println(ret)
}
```

#### [JZ63-简单] 买卖股票的最好时机(一)

##### 描述

假设你有一个数组 prices，长度为 n，其中 prices[i] 是股票在第 i 天的价格，请根据这个价格数组，返回买卖股票能获得的最大收益

1. 你可以买入一次股票和卖出一次股票，并非每天都可以买入或卖出一次，总共只能买入和卖出一次，且买入必须在卖出的前面的某一天
2. 如果不能获取到任何利润，请返回0
3. 假设买入卖出均无手续费

数据范围： 0≤ n≤10^5 , 0≤val≤10^4

要求：空间复杂度 O(1)，时间复杂度 O(n)

##### 示例

输入：

```
// 输入
[8,9,2,5,4,7,1]
// 输出
5
// 说明：在第3天(股票价格 = 2)的时候买入，在第6天(股票价格 = 7)的时候卖出，最大利润 = 7-2 = 5 ，不能选择在第2天买入，第3天卖出，这样就亏损7了；同时，你也不能在买入前卖出股票。 

// 输入
[2,4,1]
// 返回值
2

// 输入
[3,2,1]
// 返回值
0
```

##### 解题思路

动态规划中贪心算法：找出整体当中给的每个局部子结构的最优解，并且最终将所有的这些局部最优解结合起来形成整体上的一个最优解。

- 将每天收入与最低价格相见获取最大值

##### 代码实现

```go
/**
 * [JZ63-简单] 买卖股票的最好时机(一)
 *
 * @param array int 一维数组
 * @return int
 */
func maxProfit(prices []int) int {
	n := len(prices)
	if 0 == n {
		return 0
	}

	min := prices[0]

	ret := 0
	for i := 1; i < n; i++ {
		min = int(math.Min(float64(min), float64(prices[i])))
		ret = int(math.Max(float64(ret), float64(prices[i]-min)))
	}
	return ret
}
```

#### [中等]矩形覆盖

##### 题目描述

我们可以用2 * 1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2 * 1的小矩形无重叠地覆盖一个2 * n的大矩形，总共有多少种方法？

比如n=3时，2 * 3的矩形块有3种覆盖方法：

<img src="../../Image/image-20201011192132838.png" alt="image-20201011192132838" style="zoom:50%;" />

##### 解题思路

> n：	 1	2	3	4
>
> 方法：1	2	3	5

可以发现此题也是斐波那契数列的解法

##### 代码实现

示例1（递归）：

```php
function rectCover($number)
{
    if ($number === 0 || $number === 1 || $number === 2) return $number;
    $sum = jumpFloor($number - 1) + jumpFloor($number - 2);
    return $sum;
}
```

示例2：

```php
function rectCover($number)
{
    if ($number === 0 || $number === 1 || $number === 2) return $number;
    $pre = 1;
    $next = 2;
    for ($i = 3; $i <= $number; $i++) {
    	$sum = $pre + $next;
    	$pre = $next;
    	$next = $sum;
    }
    return $sum;
}
```

#### [JZ47-中等] 礼物的最大价值

##### 描述

在一个m*×*n的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

如输入这样的一个二维数组，
[
[1,3,1],
[1,5,1],
[4,2,1]
]

那么路径 1→3→5→2→1 可以拿到最多价值的礼物，价值为12

- 0<grid.length≤200 
- 0<grid[0].length≤200

##### 示例

```
// 输入：
[[1,3,1],[1,5,1],[4,2,1]]
// 返回值：
12
```

##### 解题思路

动态规划

##### 代码实现

```go
/**
 * 动态规划
 *
 * @param grid int整型二维数组
 * @return int整型
 */
func maxValue(grid [][]int) int {
	// n 行，m 列
	n, m := len(grid), len(grid[0])

	for i := 0; i < n; i++ {
		for j := 0; j < m; j++ {
			if i > 0 && j > 0 {
				grid[i][j] += int(math.Max(float64(grid[i-1][j]), float64(grid[i][j-1])))
			} else if i > 0 && j == 0 {
				grid[i][j] += grid[i-1][j]
			} else if i == 0 && j > 0 {
				grid[i][j] += grid[i][j-1]
			}
		}
	}

	return grid[n-1][m-1]
}
```

#### [JZ48-中等] 最长不含重复字符的子字符串

##### 描述

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

数据范围：s.length≤40000

##### 示例

```
// 输入：
"abcabcbb"
// 返回值：
3
// 说明：因为无重复字符的最长子串是"abc"，所以其长度为 3。

// 输入：
"bbbbb"
// 返回值：
1
说明：因为无重复字符的最长子串是"b"，所以其长度为 1。

// 输入：
"pwwkew"
// 返回值：
3
// 说明：因为无重复字符的最长子串是 "wke"，所以其长度为 3。
// 请注意，你的答案必须是子串的长度，"pwke" 是一个子序列，不是子串。
```

##### 解题思路

滑动窗口

通过 left、right 指针控制左右窗口，长度等于 right - left + 1，右指针依次向右移动

通过 hash（或数组）记录每个字符出现的次数，如果大于 1，则左窗口依次向右移动，直到该字符等于 1 为止

##### 代码实现

```go
/**
 * 滑动窗口
 *
 * @param s string 字符串
 * @return int 整型
 */
func lengthOfLongestSubstring(s string) int {
	n := len(s)
	if n == 1 {
		return 1
	}

	hash := make([]int, 256)
	left, right, max := 0, 0, 1
	for right < n {
		hash[s[right]] += 1
		for hash[s[right]] != 1 {
			hash[s[left]]--
			left++
		}
		max = int(math.Max(float64(max), float64(right-left+1)))
		right++
	}

	return max
}
```

### 回溯算法

#### [JZ12-中等] 矩阵中的路径

##### 描述

请设计一个函数，用来判断在一个 n 乘 m 的矩阵中是否存在一条包含某长度为 len 的字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 

数据范围：0≤*n*，m≤20 ，≤len≤25 

##### 示例

```
// 输入：
[[a,b,c,e],[s,f,c,s],[a,d,e,e]],"abcced"
// 返回值：
true

// 输入：
[[a,b,c,e],[s,f,c,s],[a,d,e,e]],"abcb"
// 返回值：
false
```

##### 解题思路

递归

##### 代码实现

```go
package main

import "fmt"

func dfsMatrix(matrix [][]byte, n int, m int, i int, j int, word string, k int, flag [][]bool) bool {
	// i, j 越界，或字符不匹配、或该点已走过则退出
	if i < 0 || i >= n || j < 0 || j >= m || (matrix[i][j] != word[k]) || flag[i][j] == true {
		return false
	}

	// 已经找到
	if k == len(word)-1 {
		return true
	}

	// 标记当前位置已经走过
	flag[i][j] = true

	// 向四个方向查找
	if dfsMatrix(matrix, n, m, i-1, j, word, k+1, flag) == true ||
		dfsMatrix(matrix, n, m, i+1, j, word, k+1, flag) == true ||
		dfsMatrix(matrix, n, m, i, j-1, word, k+1, flag) == true ||
		dfsMatrix(matrix, n, m, i, j+1, word, k+1, flag) == true {
		return true
	}

	flag[i][j] = false
	return false
}

/**
 * [JZ12-中等] 矩阵中的路径
 *
 * @param matrix char 字符型二维数组
 * @param word string 字符串
 * @return bool 布尔型
 */
func hasPath(matrix [][]byte, word string) bool {
	if len(matrix) == 0 {
		return false
	}

	// 矩阵 n、m
	n, m := len(matrix), len(matrix[0])

	// 初始化一个标记矩阵，记录矩阵走过路径
	flag := make([][]bool, n)
	for k := 0; k < n; k++ {
		flag[k] = make([]bool, m)
	}

	// 遍历矩阵，查找以每个点为起始点情况下是否存在该路径
	for i := 0; i < n; i++ {
		for j := 0; j < m; j++ {
			// 查找路径
			if dfsMatrix(matrix, n, m, i, j, word, 0, flag) == true {
				return true
			}
		}
	}

	return false
}

func main() {
	fmt.Println(hasPath([][]byte{{'a', 'b', 'c', 'e'}, {'s', 'f', 'c', 's'}, {'a', 'd', 'e', 'e'}}, "abcced"))
}
```

### 排序算法

#### [JZ3-简单] 数组中重复的数字

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

##### 解题思路

1.暴力

2.位置重排

数组长度为 n 只包含了 0 到 n−1 的数字，那么如果数字没有重复，这些数字排序后将会与其下标一一对应。那我们就可以考虑遍历数组，每次检查数字与下标是不是一致的，一致的说明它在属于它的位置上，不一致我们就将其交换到该数字作为下标的位置上，如果交换过程中，那个位置已经出现了等于它下标的数字，那肯定就重复了。

##### 代码实现

暴力：

```go
/**
 * 【暴力】数组中重复的数字
 *
 * @param numbers int 整型一维数组
 * @return int 整型
 */
func duplicate(numbers []int) int {
	// 遍历数组
	for i := 0; i < len(numbers)-1; i++ {
		for j := i + 1; j < len(numbers); j++ {
			if numbers[i] == numbers[j] {
				return numbers[i]
			}
		}
	}
	return -1
}
```

位置重排：

```go
/**
 * 位置重排
 *
 * @param numbers int 整型一维数组
 * @return int 整型
 */
func duplicate2(numbers []int) int {
	for i := 0; i < len(numbers); i++ {
		if numbers[i] != i {
			// 当前值对应的下标已经有正确的对应数值，说明重复
			if numbers[i] == numbers[numbers[i]] {
				return numbers[i]
			}
			// 未存在，则交换位置
			numbers[i], numbers[numbers[i]] = numbers[numbers[i]], numbers[i]
		}
	}
	return -1
}
```

#### [JZ40-中等] 最小的K个数

##### 描述

给定一个长度为 n 的可能有重复值的数组，找出其中不去重的最小的 k 个数。例如数组元素是 4,5,1,6,2,7,3,8 这 8 个数字，则最小的4个数字是1,2,3,4(任意顺序皆可)。

数据范围：0≤*k*,*n*≤10000，数组中每个数的大小0 ≤val≤1000

要求：空间复杂度 O(n) ，时间复杂度 O(nlogn)

##### 示例

```
// 输入：
[4,5,1,6,2,7,3,8],4 
// 返回值：
[1,2,3,4]
// 说明：返回最小的4个数即可，返回[1,3,2,4]也可以

// 输入：
[1],0
// 返回值：
[]

// 输入：
[0,1,2,1,2],3
// 返回值：
[0,1,1]
```

##### 解题思路

- 排序
- 取排序后数组前 4 位

##### 代码实现

冒泡排序：

```go
/**
 * 冒泡排序
 *
 * @param input int 整型一维数组
 * @param k int 整型
 * @return int 整型一维数组
 */
func getLeastNumbers(input []int, k int) []int {
	result := []int{}
	if k == 0 {
		return result
	}

	count := len(input)
	if count < k {
		return result
	}

	for i := 0; i < count; i++ {
		for j := 0; j < count-1; j++ {
			if input[j] > input[j+1] {
				input[j], input[j+1] = input[j+1], input[j]
			}
		}
	}
	result = input[:k]

	return result
}
```

快排：

```go
func quickSort(arr []int, left int, right int) {
	if left > right {
		return
	}

	tmp := arr[left]
	i := left
	j := right

	for i != j {
		for arr[j] >= tmp && i < j {
			j--
		}
		for arr[i] <= tmp && i < j {
			i++
		}
		if i < j {
			arr[i], arr[j] = arr[j], arr[i]
		}
	}
	arr[left], arr[i] = arr[i], arr[left]

	quickSort(arr, left, i-1)
	quickSort(arr, j+1, right)
}

/**
 * 快排
 *
 * @param input int 整型一维数组
 * @param k int 整型
 * @return int 整型一维数组
 */
func getLeastNumbers2(input []int, k int) []int {
	result := []int{}
	if k == 0 {
		return result
	}

	count := len(input)
	if count < k {
		return result
	}

	quickSort(input, 0, count-1)
	result = input[:k]

	return result
}
```

### 位运算

位运算符简介：

| 例子             | 名称              | 结果                                 |
| :------------- | :-------------- | :--------------------------------- |
| **`$a & $b`**  | And（按位与）        | 将把 $a 和 $b 中都为 1 的位设为 1。           |
| **`$a | $b`**  | Or（按位或）         | 将把 $a 和 $b 中任何一个为 1 的位设为 1。        |
| **`$a ^ $b`**  | Xor（按位异或）       | 将把 $a 和 $b 中一个为 1 另一个为 0 的位设为 1。   |
| **`~ $a`**     | Not（按位取反）       | 将 $a 中为 0 的位设为 1，反之亦然。             |
| **`$a << $b`** | Shift left（左移）  | 将 $a 中的位向左移动 $b 次（每一次移动都表示“乘以 2”）。 |
| **`$a >> $b`** | Shift right（右移） | 将 $a 中的位向右移动 $b 次（每一次移动都表示“除以 2”）。 |

#### [简单] 不用加减乘除做加法

##### 题目描述

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

##### 示例

```
输入：
1,2
返回值：
3
```

##### 解题思路

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

##### 代码实现

PHP

```php
<?php

function Add($num1, $num2)
{
    if($num1 == 0) return $num2;
    if($num2 == 0) return $num1;
    return Add($num1 ^ $num2, ($num1 & $num2) << 1);
}
```

Golang

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

#### [简单] 二进制中1的个数

##### 题目描述

输入一个整数，输出该数32位二进制表示中1的个数。其中负数用补码表示。

##### 代码实现

示例1：

```javascript
function NumberOf1(n)
{
    let flag = 1;
    let count = 0;
    while(flag){
        if (flag & n) {
            count++;
        }
        flag <<= 1;
    }
    return count
}
```

示例2：

```php
<?php

function NumberOf1($n)
{
    $count = 0;
  	// 如果n小于0，php、python等需要做特殊处理
    if ($n < 0) {
        $n = $n & 0xffffffff;
    }
    while ($n != 0) {
        $count++;
        $n = $n & ($n - 1);
    }
    return $count;
}
```

```javascript
function NumberOf1(n)
{
    let count = 0;
    while (n != 0) {
        count++;
        n = n & (n-1);
    }
    return count;
}
```

##### 思路

如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。
如：一个二进制数1100，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.我们发现减1的结果是把最右边的一个1开始的所有位都取反了。这个时候如果我们再把原来的整数和减去1之后的结果做`按位与`运算，从原来整数最右边一个1那一位开始所有位都会变成0。如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做`按位与`运算，会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。

##### 知识点

进制转化

```
你以十进制的数除以你所要转换的进制数,把每次除得的余数记在旁边,所得的商数继续除以进制数,直到余数为0时止.
十进制转八进制：
100/8=12...(余数为4); 
12/8=1.....(余数为4); 
1/8=0......(余数为1); 
结果：144
十进制转十六进制: 
100/16=6....(余数为4); 
6/16=0......(余数为6); 
结果：64; 
十进制转二进制: 
100/2=50....(余数为0); 
50/2=25.....(余数为0); 
25/2=12.....(余数为1); 
12/2=6......(余数为0); 
6/2=3.......(余数为0); 
3/2=1.......(余数为1); 
1/2=0.......(余数为1); 
结果：1100100; 
```

位运算符知识

[计算机组成原理](计算机组成原理.md)

#### [中等] 数值的整数次方

##### 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

保证base和exponent不同时为0

##### 代码实现

```php
function Power($base, $exponent)
{
    if ($exponent == 0) return 1;
    if ($base == 0) return 0;
    return pow($base, $exponent);
}
```

#### [JZ51-中等] 数组中的逆序对

##### 描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对 1000000007 取模的结果输出。 即输出P mod 1000000007

数据范围： 对于 50% 的数据, size≤104
对于 100% 的数据, size≤105

数组中所有数字的值满足 0 ≤*v**a**l*≤1000000

要求：空间复杂度 O(n)，时间复杂度 O(nlogn)

输入描述：题目保证输入的数组中没有的相同的数字

##### 示例

```
// 输入：
[1,2,3,4,5,6,7,0]
// 返回值：
7

// 输入：
[1,2,3]
// 返回值：
0
```

##### 解题思路

归并排序

##### 代码实现

```go
var count int = 0

/**
 * 归并排序法
 *
 * @param data int 整型一维数组
 * @return int 整型
 */
func inversePairs(data []int) int {
	n := len(data)
	if n < 2 {
		return 0
	}

	mergeSort(data, 0, len(data)-1)

	return count
}

func mergeSort(arr []int, left int, right int) {
	if left >= right {
		return
	}

	mid := left + (right-left)/2

	mergeSort(arr, left, mid)
	mergeSort(arr, mid+1, right)

	merge(arr, left, mid, right)
}

func merge(arr []int, left int, mid int, right int) {
	result := make([]int, right-left+1)
	c, s, l, r := 0, left, left, mid+1

	for l <= mid && r <= right {
		if arr[l] <= arr[r] {
			result[c] = arr[l]
			l++
		} else {
			count += mid + 1 - l
			count %= 1000000007

			result[c] = arr[r]
			r++
		}
		c++
	}

	for l <= mid {
		result[c] = arr[l]
		c++
		l++
	}

	for r <= right {
		result[c] = arr[r]
		c++
		r++
	}

	for i := 0; i < len(result); i++ {
		arr[s] = result[i]
		s++
	}
}
```

#### [JZ56-中等] 数组中只出现一次的两个数字

##### 描述

一个整型数组里除了两个数字只出现一次，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

数据范围：数组长度 2≤*n*≤1000，数组中每个数的大小 0 < val ≤ 1000000
要求：空间复杂度 O(1)，时间复杂度 O(n)

提示：输出时按非降序排列。

##### 示例

```
// 输入：
[1,4,1,6]
// 返回值：
[4,6]
// 说明：返回的结果中较小的数排在前面

// 输入：
[1,2,3,3,2,9]
// 返回值：
[1,9]
```

##### 解题思路

哈希法：

- 先遍历数组，以数为键名，出现次数为键值存入map（也可以是 array）
- 再遍历 map，找出 val 为 1 的两个数，组成数组
- 判断两个数大小，从小到大排序

##### 代码实现

```go
/**
 * [JZ56-中等] 数组中只出现一次的两个数字
 *
 * @param array int整型一维数组
 * @return int整型一维数组
 */
func findNumsAppearOnce(array []int) []int {
	hash := map[int]int{}

	// 统计每个数出现的次数
	for _, v := range array {
		if _, ok := hash[v]; ok {
			hash[v] += 1
		} else {
			hash[v] = 1
		}
	}

	// 遍历查找次数等于 1 的数
	result := []int{}
	for k, v := range hash {
		if v == 1 {
			result = append(result, k)
		}
	}

	// 排序 - 就两个数，判断下大小即可
	for i := 0; i < len(result)-1; i++ {
		if result[i] > result[i+1] {
			result[i], result[i+1] = result[i+1], result[i]
		}
	}

	return result
}
```

#### [JZ64-中等] 求1+2+3+...+n

##### 描述

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

数据范围： 0 0<*n*≤200
进阶： 空间复杂度 O(1) ，时间复杂度 O(n)

##### 示例

```
// 输入：
5
// 返回值：
15

// 输入：
1
// 返回值：
1
```

##### 解题思路

1.递归

2.找规律：sum = n * (n + 1) / 2

##### 代码实现

Golang：

```go
/**
 * [JZ64-中等] 求1+2+3+...+n
 *
 * @param n int 整型
 * @return int 整型
 */
func sumOfOneToN(n int) int {
	return n * (n + 1) / 2
}
```

JavaScript：

```javascript
function Sum_Solution(n) {
    return n && n + Sum_Solution(n-1);
}
```

PHP：

```php
function Sum_Solution($n)
{
    $n > 1 && $n += Sum_Solution($n-1);
    return $n;
}
```

### 模拟

#### [JZ29-简单] 顺时针打印矩阵

##### 描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵：

```
[[1,2,3,4],
[5,6,7,8],
[9,10,11,12],
[13,14,15,16]]
```

则依次打印出数字

```
[1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10]
```

数据范围:

0 <= matrix.length <= 100

0 <= matrix[i].length <= 100

##### 示例

```
// 输入：
[[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]]
// 返回值：
[1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10]

// 输入：
[[1,2,3,1],[4,5,6,1],[4,5,6,1]]
// 返回值：
[1,2,3,1,1,1,6,5,4,4,5,6]
```

##### 解题思路

```
[[1,2,3,4],
[5,6,7,8],
[9,10,11,12],
[13,14,15,16]]

设左边界、右边界、上边界、下边界分别为 left、right、up、down
则：left=0、right=4=matrix[0].length-1、up=0、down=matrix.length-1
1.上边界先从左到右遍历，遍历后上边界下移（up++）
2.右边界从上往下遍历，遍历后右边界左移（right--）
3.下边界从右往左遍历，遍历后下边界上移（down--）
4.左边界从下往上遍历，遍历后左边界右移（left++）
一直重复 1-4，最后遍历完成即可
```

##### 代码实现

```go
/**
 * 边界模拟法
 *
 * @param matrix int 整型二维数组
 * @return int 整型一维数组
 */
func printMatrix(matrix [][]int) []int {
	result := []int{}
	n := len(matrix)
	if 0 == n {
		return result
	}

	// 左、右、上、下 边界
	left, right, up, down := 0, len(matrix[0])-1, 0, n-1
	for left <= right && up <= down {
		// 遍历上边界从左到右
		for i := left; i <= right; i++ {
			result = append(result, matrix[up][i])
		}
		// 上边界向下
		up++
		if up > down {
			break
		}

		// 右边界的从上到下
		for i := up; i <= down; i++ {
			result = append(result, matrix[i][right])
		}
		//右边界向左
		right--
		if left > right {
			break
		}

		// 下边界的从右到左
		for i := right; i >= left; i-- {
			result = append(result, matrix[down][i])
		}
		// 下边界向上
		down--
		if up > down {
			break
		}

		// 左边界从下向上
		for i := down; i >= up; i-- {
			result = append(result, matrix[i][left])
		}
		// 左边界向右
		left++
		if left > right {
			break
		}
	}

	return result
}
```

#### [JZ61-简单] 扑克牌顺子

##### 描述

现在有2副扑克牌，从扑克牌中随机五张扑克牌，我们需要来判断一下是不是顺子。
有如下规则：

1. A为1，J为11，Q为12，K为13，A不能视为14
2. 大、小王为 0，0可以看作任意牌
3. 如果给出的五张牌能组成顺子（即这五张牌是连续的）就输出true，否则就输出false。
4. 数据保证每组5个数字，每组最多含有4个零，数组的数取值为 [0, 13]

要求：空间复杂度 O(1)，时间复杂度 O(nlogn)，本题也有时间复杂度 O(n) 的解法

##### 示例

```
// 输入：
[6,0,2,0,4]
// 返回值：
true
//说明：中间的两个0一个看作3，一个看作5 。即：[6,3,2,5,4] 这样这五张牌在[2,6]区间连续，输出true

// 输入：
[0,3,2,6,4]
// 返回值：
true

// 输入：
[1,0,0,1,0]
// 返回值：
false

// 输入：
[13,12,11,0,1]
// 返回值：
false
```

##### 解题思路

五个连续的数，则说明：

- 5 个数中一定不存在相同的数
- 5 个数中一定不存在两个数相差大于 4 的数（排除 0 情况）

##### 代码实现

```go
/**
 * [JZ61-简单] 扑克牌顺子
 *
 * @param numbers int整型一维数组
 * @return bool布尔型
 */
func isContinuous(numbers []int) bool {
	for i := 0; i < len(numbers)-1; i++ {
		for j := i + 1; j < len(numbers); j++ {
			if numbers[i] == 0 || numbers[j] == 0 {
				continue
			}
			tmp := numbers[i] - numbers[j]
			// 存在两数相等或者相差大于 4，则肯定不连续
			if tmp > 4 || tmp < -4 || tmp == 0 {
				return false
			}
		}
	}
	return true
}
```

#### [JZ67-中等] 把字符串转换成整数(atoi)

##### 描述

写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。传入的字符串可能有以下部分组成:

1. 若干空格
2. （可选）一个符号字符（'+' 或 '-'）
3. 数字，字母，符号，空格组成的字符串表达式
4. 若干空格

**转换算法如下:**

- 去掉无用的前导空格
- 第一个非空字符为+或者-号时，作为该整数的正负号，如果没有符号，默认为正数
- 判断整数的有效部分：
  - 确定符号位之后，与之后面尽可能多的连续数字组合起来成为有效整数数字，如果没有有效的整数部分，那么直接返回0
  - 将字符串前面的整数部分取出，后面可能会存在存在多余的字符(字母，符号，空格等)，这些字符可以被忽略，它们对于函数不应该造成影响
  - 整数超过 32 位有符号整数范围 [−231, 231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231的整数应该被调整为 −231 ，大于 231 − 1 的整数应该被调整为 231 − 1
- 去掉无用的后导空格

数据范围:

1. 0 <=字符串长度<= 100
2. 字符串由英文字母（大写和小写）、数字（0-9）、' '、'+'、'-' 和 '.' 组成

##### 示例

```
// 输入：
"82"
// 返回值：
82

// 输入：
"   -12  "
// 返回值：
-12
// 说明：去掉前后的空格，为-12

// 输入：
"4396 clearlove"
// 返回值：
4396
// 说明：6后面的字符不属于有效的整数部分，去除，但是返回前面提取的有效部分

// 输入：
"clearlove 4396"
// 返回值：
0

// 输入：
"-987654321111"
// 返回值：
-2147483648
```

##### 解题思路

- 去掉前后空格
- 处理符号位
- 判断整数有效部分
- 结果数据范围判断

##### 代码实现

```go
/**
 * [JZ67-中等] 把字符串转换成整数(atoi)
 *
 * @param s string字符串
 * @return int整型
 */
func strToInt(s string) int {
	// 去掉前后空格
	s = strings.Trim(s, " ")
	if len(s) == 0 {
		return 0
	}

	// 处理符号位
	sign := 1
	if s[0] == '-' {
		sign = -1
		s = s[1:]
	} else if s[0] == '+' {
		s = s[1:]
	}

	// 判断整数有效部分
	res := 0
	for _, v := range s {
		// 数据范围
		if v >= '0' && v <= '9' {
			// 将符合条件的整数字符串转化后加入的结果整数中
			res = res*10 + int(v-'0')
		} else {
			break
		}
		if res > int(math.Pow(2, 31)) {
			break
		}
	}
	// 乘以符号位
	res = int(sign) * res

	// 数据范围判断
	if res > int(math.Pow(2, 31)-1) {
		return int(math.Pow(2, 31) - 1)
	}
	if res < int(0-math.Pow(2, 31)) {
		return int(0 - math.Pow(2, 31))
	}

	return int(res)
}
```

### 其他算法

#### [简单] 构建乘积数组

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

PHP

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

Golang

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

Golang

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

#### [简单] 替换空格

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

PHP

```php
<?php
function replaceSpace($str)
{
    return str_replace(" ", "%20", $str);
}
```

Golang

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

#### [简单] 第一个只出现一次的字符串

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

##### 解题思路

先开一个map数组统计字符串每个元素出现的次数，再遍历字符串，如果map中的某个元素值为1，代表出现1次，直接返回

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

#### [JZ39-简单] 数组中出现次数超过一半的数字

##### 描述

给一个长度为 n 的数组，数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

例如输入一个长度为 9 的数组 [1,2,3,2,2,2,5,4,2] 。由于数字 2 在数组中出现了 5 次，超过数组长度的一半，因此输出2。

- 数据范围：n≤50000，数组中元素的值 0≤val≤10000
- 要求：空间复杂度：O(1)，时间复杂度 O(n)
- 保证数组输入非空，且保证有解

##### 示例

```
// 输入：
[1,2,3,2,2,2,5,4,2]
// 返回值：
2

// 输入：
[3,3,3,3,2,2,2]
// 返回值：
3

// 输入：
[1]
// 返回值：
1
```

##### 解题思路

哈希法

- 先遍历一遍数组，将每个元素出现的次数存在入 map 中，再遍历 map，找出次数大于数组一半的数

候选法（优化）

- 如果两个数不相等，就消去这两个数，最坏情况下，每次消去一个众数和一个非众数，那么如果存在众数，最后留下的数肯定是众数。

##### 代码实现

哈希法：

```go
/**
 * [JZ39-简单] 数组中出现次数超过一半的数字（哈希法）
 *
 * @param numbers int整型一维数组
 * @return int整型
 */
func moreThanHalfNum(numbers []int) int {
	length := len(numbers)
	if 1 == length {
		return numbers[0]
	}

	hash := map[int]int{}
	for _, v := range numbers {
		if _, ok := hash[v]; ok {
			hash[v] += 1
		} else {
			hash[v] = 1
		}
	}

	for k, v := range hash {
		if v > length/2 {
			return k
		}
	}

	return 0
}
```

候选法：

```go
/**
 * [JZ39-简单] 数组中出现次数超过一半的数字（候选法）
 *
 * @param numbers int整型一维数组
 * @return int整型
 */
func moreThanHalfNum(numbers []int) int {
	length := len(numbers)
	if 1 == length {
		return numbers[0]
	}

	cond, count := -1, 0
	for _, v := range numbers {
		if 0 == count {
			cond = v
			count++
		} else {
			if cond == v {
				count++
			} else {
				count--
			}
		}
	}

	count = 0
	for _, v := range numbers {
		if v == cond {
			count++
		}
	}

	if count > length/2 {
		return cond
	} else {
		return 0
	}
}
```

#### [JZ17-简单] 打印从1到最大的n位数

##### 描述

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

- 用返回一个整数列表来代替打印
- n 为正整数，0 < n <= 5

##### 示例

```
// 输入：
1
// 返回值：
[1,2,3,4,5,6,7,8,9]
```

##### 解题思路

由题意可知，数组长度为 10^n - 1，先求出数组长度，再遍历依次向空数组中添加元素即可

##### 代码实现

```go
/**
 * [JZ17-简单] 打印从1到最大的n位数
 *
 * @param n int整型 最大位数
 * @return int整型一维数组
 */
func printNumbers(n int) []int {
	pow := math.Pow10(n)
	count := int(pow) - 1
	arr := make([]int, count)
	for i := 1; i <= count; i++ {
		arr[i-1] = i
	}
	return arr
}
```

#### [JZ21-简单] 调整数组顺序使奇数位于偶数前面(一)

##### 描述

输入一个长度为 n 整数数组，数组里面可能含有相同的元素，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前面部分，所有的偶数位于数组的后面部分，对奇数和奇数，偶数和偶数之间的相对位置不变

数据范围：0≤*n*≤50000，数组中每个数的值 0≤*v**a**l*≤10000

要求：时间复杂度 O(n)，空间复杂度 O(1)

##### 示例

```
// 输入
[1,2,3,4]
// 输出
[1,3,2,4]

// 输入
[1,3,5,6,7]
// 输出
[1,3,5,7,6]

// 输入
[1,4,4,3]
// 输出
[1,3,4,4]
```

##### 解题思路

双指针

- 先遍历数组获取奇数个数 odd，也就是偶数在新数组起点位置
- 双指针 left、right，奇数在 0 位置后添加，偶数在 odd 位置后添加

##### 代码实现

```go
/**
 * [JZ21-简单] 调整数组顺序使奇数位于偶数前面(二)
 *
 * @param array int 整型一维数组
 * @return int 整型一维数组
 */
func reOrderArrayTwo2(array []int) []int {
	n := len(array)
	if n <= 1 {
		return array
	}

	odd := 0
	for _, v := range array {
		if v%2 == 1 {
			odd++
		}
	}

	left, right := 0, odd
	arr := make([]int, n)
	for i := 0; i < n; i++ {
		if array[i]%2 == 1 {
			arr[left] = array[i]
			left++
		} else {
			arr[right] = array[i]
			right++
		}
	}
	return arr
}
```

#### [JZ81-简单] 调整数组顺序使奇数位于偶数前面(二)

##### 描述

输入一个长度为 n 整数数组，数组里面可能含有相同的元素，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前面部分，所有的偶数位于数组的后面部分，对奇数和奇数，偶数和偶数之间的相对位置不做要求，但是时间复杂度和空间复杂度必须如下要求。

数据范围：0≤*n*≤50000，数组中每个数的值 0≤*v**a**l*≤10000

要求：时间复杂度 O(n)，空间复杂度 O(1)

##### 示例

```
// 输入
[1,2,3,4]
// 输出
[1,3,2,4]
// 说明
[3,1,2,4]或者[3,1,4,2]也是正确答案 

// 输入
[1,3,5,6,7]
// 输出
[1,3,5,7,6]
// 说明
[3,1,5,7,6]等也是正确答案 

// 输入
[1,4,4,3]
// 输出
[1,3,4,4]
```

##### 代码实现

遍历数组，将奇数添加到新数组头部，偶数 append 到数组尾部

```go
/**
 * [JZ81-简单] 调整数组顺序使奇数位于偶数前面(二)
 *
 * @param array int 整型一维数组
 * @return int 整型一维数组
 */
func reOrderArrayTwo(array []int) []int {
	n := len(array)
	if n <= 1 {
		return array
	}

	var arr []int
	for _, v := range array {
		if v%2 == 1 {
			arr = append([]int{v}, arr...)
		} else {
			arr = append(arr, v)
		}
	}
	return arr
}
```

双指针（优化）

- 先遍历数组获取奇数个数 odd，也就是偶数在新数组起点位置
- 双指针 left、right，奇数在 0 位置后添加，偶数在 odd 位置后添加

```go
/**
 * [JZ81-简单] 调整数组顺序使奇数位于偶数前面(二)
 *
 * @param array int 整型一维数组
 * @return int 整型一维数组
 */
func reOrderArrayTwo2(array []int) []int {
	n := len(array)
	if n <= 1 {
		return array
	}

	odd := 0
	for _, v := range array {
		if v%2 == 1 {
			odd++
		}
	}

	left, right := 0, odd
	arr := make([]int, n)
	for i := 0; i < n; i++ {
		if array[i]%2 == 1 {
			arr[left] = array[i]
			left++
		} else {
			arr[right] = array[i]
			right++
		}
	}
	return arr
}
```

#### [JZ43-中等] 从1到n整数中1出现的次数

##### 描述

输入一个整数 n ，求 1～n 这 n 个整数的十进制表示中 1 出现的次数
例如， 1~13 中包含 1 的数字有 1 、 10 、 11 、 12 、 13 因此共出现 6 次

注意：11 这种情况算两次

数据范围： 1≤*n*≤30000 

进阶：空间复杂度 O(1)  ，时间复杂度 O(lognn)

##### 示例

```
// 输入：
13
// 返回值：
6

// 输入：
0
// 返回值：
0
```

##### 解题思路

最简单的方法是依次遍历，判断每个数中存在的 1 的数量相加，但是复杂度较大

仔细观察本题，可以发现其实是一道数学找规律的题目

首先，如果我们修改下题目，如果要找出 1 到 n 中百位数上 1 出现的次数？那么，则可以推导：

```
// 假设：n = 12013
12013			12113			12213			12313
------			------			------			------
100 ~ 199		100 ~ 199		100 ~ 199		100 ~ 199
1100 ~ 1199		1100 ~ 1199		1100 ~ 1199		1100 ~ 1199
2100 ~ 2199		2100 ~ 2199		2100 ~ 2199		2100 ~ 2199
...				...				...				...
9100 ~ 9199		9100 ~ 9199		9100 ~ 9199		9100 ~ 9199
10100 ~ 10199	10100 ~ 10199	10100 ~ 10199	10100 ~ 10199
11100 ~ 11199	11100 ~ 11199	11100 ~ 11199	11100 ~ 11199
				12100 ~ 12113	12100 ~ 12199	12100 ~ 12199
+				+				+				+
------			------			------			------
12*100			12*100+13+1		(12+1)*100		(12+1)*100

// 从上推到可以发现：百位上 1 出现的次数受百位前的高位 12 个低位 13 的影响，规律如下：
假设：高位=high 当前百位=cur 低位=low
- 当 cur == 0 时：count = high * 100
- 当 cur == 1 时：count = high * 100 + low + 1
- 当 cur > 1 时：count = (high + 1) * 100
```

已知百位上 1 出现的次数，则可以将百位位数左右移动，也可以知道各位、十位中 1 出现的次数，将以上的 100 改为 i*10（1、10、100、...）

则有：

```
- 当 cur == 0 时：count = high * i * 10
- 当 cur == 1 时：count = high * i * 10 + low + 1
- 当 cur > 1 时：count = (high + 1) * i *10)
```

##### 代码实现

```go
/**
 * [JZ43-中等] 从 1 到 n 整数中 1 出现的次数
 *
 * @param n int整型
 * @return int整型
 */
func numOfOneFromInt(n int) int {
	i, count, high, cur, low := 1, 0, 0, 0, 0
	for i <= n {
		// 取高位
		high = n / (i * 10)
		// 取当前位
		cur = (n / i) % 10
		// 取低位
		low = n - n/i*i
		if cur == 0 {
			count += high * i
		} else if cur == 1 {
			count += high*i + low + 1
		} else {
			count += (high + 1) * i
		}
		i *= 10
	}
	return count
}
```

#### [JZ58-中等] 左旋转字符串

##### 题目描述

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

##### 解题思路

字符串操作

##### 代码实现

PHP：

```php
function LeftRotateString($str, $n)
{
    if(strlen($str) == 0) return $str;
    $str1 = substr($str,0,$n);
    $str2 = substr($str,$n);
    return $str2.$str1;
}
```

Node：

```javascript
function LeftRotateString(str, n)
{
    if(!str || str.length === 0) return '';
    let num = n % str.length;
    return str.substr(num) + str.substr(0,num);
}
```

Golang：

```go
/**
 * [JZ58-中等] 左旋转字符串
 *
 * @param str string 字符串
 * @param n int 整型
 * @return string 字符串
 */
func leftRotateString(str string, n int) string {
	if 0 == len(str) {
		return ""
	}

	n = n % len(str)

	return str[n:] + str[:n]
}
```

#### [JZ45-中等] 把数组排成最小的数

##### 题目描述

输入一个非负整数数组 numbers，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

例如输入数组[3，32，321]，则打印出这三个数字能排成的最小数字为321323。

1.输出结果可能非常大，所以你需要返回一个字符串而不是整数
2.拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0

数据范围：0<=len(numbers)<=100

##### 示例

```
// 输入：
[11,3]
// 返回值：
"113"

// 输入：
[]
// 返回值：
""

// 输入：
[3,32,321]
// 返回值：
"321323"
```

##### 解题思路

- 对 numbers 中的元素（x、y）进行排序，排序规则是：x+y 和 y+x 比较大小（x、y需要转化成 string）
- 然后再次遍历 number，按顺序拼接成数组即可

##### 代码实现

```go
/**
 * [JZ45-中等] 把数组排成最小的数
 *
 * @param numbers int整型一维数组
 * @return string字符串
 */
func printMinNumFromArray(numbers []int) string {
	count := len(numbers)
	if count == 0 {
		return ""
	}

	for i := 0; i < count; i++ {
		for j := 0; j < count-1; j++ {
			str1 := strconv.Itoa(numbers[j]) + strconv.Itoa(numbers[j+1])
			str2 := strconv.Itoa(numbers[j+1]) + strconv.Itoa(numbers[j])
			if str1 > str2 {
				numbers[j], numbers[j+1] = numbers[j+1], numbers[j]
			}
		}
	}

	result := ""
	for _, v := range numbers {
		result += strconv.Itoa(v)
	}

	return result
}
```

#### [JZ49-中等] 丑数

##### 描述

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第 n个丑数。

数据范围：0≤*n*≤2000

要求：空间复杂度 O(n)， 时间复杂度 O(n)

##### 示例

```
// 输入：
7
// 返回值：
8
```

##### 解题思路

首先可以发现丑数存在的规律

- 前 20 个丑数：1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, 16, 18, 20, 24, 25, 27, 30, 32, 36
- 丑数：u = 2^a * 3^b * 5^c

```
将所有丑数按顺序存入数组 data 中
第一个丑数为 1，则：data[0] = 1
下一个丑数是：data[0]*2 data[0]*3 data[0]*5 中的最小的一个，即 data[0]*2=2
所以，下一个丑数为：x=data[a] * 2	y=data[b] * 3	z=data[c] * 5 中最小的一个
因为 2 已经乘了一次，所以 a 从 0 变为 1（a++）
```

##### 代码实现

```go
/**
 * [JZ49-中等] 丑数
 *
 * @param index int整型
 * @return int整型
 */
func getUglyNumber(index int) int {
	if index <= 6 {
        return index
    }
 
    a, b, c := 0, 0, 0
 
    data := make([]int, index)
    data[0] = 1
     
    for i := 1; i < index; i++ {
        min := data[a] * 2
        if t := data[b] * 3; t < min {
            min = t
        }
        if t := data[c] * 5; t < min {
            min = t
        }
        data[i] = min
        if min == data[a] * 2 {
            a++
        }
        if min == data[b] * 3 {
            b++
        }
        if min == data[c] * 5 {
            c++
        }
    }
    return data[index-1]
}
```

#### [JZ74-中等] 和为S的连续正数序列

##### 描述

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列?

- 数据范围：0 < n ≤ 100
- 进阶：时间复杂度 O(n)
- 返回值：输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

##### 示例

```
// 输入：
9
// 返回值：
[[2,3,4],[4,5]]

// 输入：
0
// 返回值：
[]
```

##### 解题思路

枚举法：

- 因为序列至少两个数，每次枚举区间的起始数字最多到目标数的一半向下取整即可，因为两个大于目标数一半的数相加一定大于目标数。
- 第一个数从 i=1 开始，依次加上后面连续的整数，相加如果大于预期值，则说明从 i=1 开始不存在，退出，在从 i=2 开始，以此枚举，找到等于预期值的连续数后，存入二维数组，再 i++ 后继续查找

滑动窗口：

连续正数序列存在以下规律：

假设首位为 l，最后一位为 r，则 s=(l + r) * (r - l + 1) / 2

因此，可以：

- 先定义一个两位数的区间 [1, 2]
- 若区间所有整数和小于预期 sum，则区间向右扩大（r++）
- 若区间所有整数和大于预期 sum，则区间向左缩小（l++）
- 直到找出等于 sum 为止（l < r）

##### 代码实现

枚举法：

```go
/**
 * 枚举法
 *
 * @param sum int整型
 * @return int整型二维数组
 */
func findContinuousSequence(sum int) [][]int {
	if sum == 0 {
		return [][]int{{}}
	}

	half := math.Ceil(float64(sum) / 2)
	count := int(half)

	result := make([][]int, 0)
	for i := 1; i < count; i++ {
		tmpArr := []int{i}
		tmpSum := i
		for j := i + 1; j <= count; j++ {
			tmpArr = append(tmpArr, j)
			tmpSum += j
			if tmpSum > sum {
				break
			} else if tmpSum == sum {
				result = append(result, tmpArr)
			}
		}
	}
	return result
}
```

滑动窗口：

```go
/**
 * 滑动窗口
 *
 * @param sum int 整型
 * @return int 整型二维数组
 */
func findContinuousSequence2(sum int) [][]int {
	if sum == 0 {
		return [][]int{{}}
	}

	result := make([][]int, 0)

	// 定义左右区间初始值为 1、2
	l, r := 1, 2
	for l < r {
		// 区间所有数之和
		tmpSum := (l + r) * (r - l + 1) / 2

		if tmpSum == sum {
			// 等于预期值，将区间处理成一维数组
			tmpArr := []int{}
			for i := l; i <= r; i++ {
				tmpArr = append(tmpArr, i)
			}
			result = append(result, tmpArr)

			// 移动区间左临界值，继续查找下一个
			l++
		} else if tmpSum < sum {
			// 小于预期值，左区间 l 右移一位
			r++
		} else if tmpSum > sum {
			// 大于预期值，右区间 r 左边移一位
			l++
		}
	}
	return result
}
```

#### [JZ57-中等] 和为S的两个数字

##### 描述

输入一个升序数组 array 和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，返回任意一组即可，如果无法找出这样的数字，返回一个空数组即可。

数据范围: 0≤len(array)≤10^5 ， 1 ≤ array[i] ≤ 10^6

##### 示例

```
// 输入：
[1,2,4,7,11,15],15
// 返回值：
[4,11]
// 说明：返回[4,11]或者[11,4]都是可以的

// 输入：
[1,5,11],10
// 返回值：
[]
// 说明：不存在，返回空数组
```

##### 解题思路

双指针，从首位下标 l=0，最后一位下标 r=len(array)-1 开始，若两数相加大于预期 sum，则 r 左移（r--），若大于预期 sum，则 l 右移（l++），找到等于 sum 的退出

##### 代码实现

```go
/**
 * 双指针
 *
 * @param array int 整型一维数组
 * @param sum int 整型
 * @return int 整型一维数组
 */
func findNumbersWithSum(array []int, sum int) []int {
	result := []int{}
	l, r := 0, len(array)-1
	for l < r {
		tmpSum := array[l] + array[r]
		if tmpSum == sum {
			result = []int{array[l], array[r]}
			break
		} else if tmpSum < sum {
			l++
		} else {
			r--
		}
	}
	return result
}
```

#### [JZ62-中等] 孩子们的游戏(圆圈中最后剩下的数)

##### 描述

每年六一儿童节，牛客都会准备一些小礼物和小游戏去看望孤儿院的孩子们。其中，有个游戏是这样的：首先，让 n 个小朋友们围成一个大圈，小朋友们的编号是0~n-1。然后，随机指定一个数 m ，让编号为0的小朋友开始报数。每次喊到 m-1 的那个小朋友要出列唱首歌，然后可以在礼品箱中任意的挑选礼物，并且不再回到圈中，从他的下一个小朋友开始，继续0... m-1报数....这样下去....直到剩下最后一个小朋友，可以不用表演，并且拿到牛客礼品，请你试着想下，哪个小朋友会得到这份礼品呢？

数据范围：1≤*n*≤5000，1≤*m*≤10000

要求：空间复杂度 O(1)，时间复杂度 O(n)

##### 示例

```
// 输入：
5,3
// 返回值：
3

// 输入：
2,3
// 返回值：
1
// 说明：有2个小朋友编号为0，1，第一次报数报到3的是0号小朋友，0号小朋友出圈，1号小朋友得到礼物

// 输入：
10,17
// 返回值：
2
```

##### 代码实现

```go
/**
 * [JZ62-中等] 孩子们的游戏(圆圈中最后剩下的数)
 *
 * @param n int 整型
 * @param m in t整型
 * @return int 整型
 */
func lastRemaining(n int, m int) int {
	x := 0
	for i := 2; i <= n; i++ {
		x = (m + x) % i
	}
	return x
}
```

#### [JZ14-中等] 剪绳子

##### 描述

给你一根长度为 n 的绳子，请把绳子剪成整数长的 m 段（ m 、 n 都是整数， n > 1 并且 m > 1 ， m <= n ），每段绳子的长度记为 k[1],...,k[m] 。请问 k[1]*k[2]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是 8 时，我们把它剪成长度分别为 2、3、3 的三段，此时得到的最大乘积是 18 。

数据范围： 2≤*n*≤60
进阶：空间复杂度 O(1)，时间复杂度 O(n)

输入描述：输入一个数n，意义见题面。

##### 示例

```
// 输入：
8
// 返回值：
18
// 说明：
8 = 2 +3 +3 , 2*3*3=18

// 输入：
2
// 返回值：
1
// 说明：
m>1，所以切成两段长度是1的绳子
```

##### 代码实现

```go
/**
 * 动态规划
 *
 * @param n int 整型
 * @return int 整型
 */
func cutRope(n int) int {
	if n <= 3 {
		return n - 1
	}

	dp := make([]int, n+1)
	dp[1] = 1
	dp[2] = 2
	dp[3] = 3
	dp[4] = 4
	for i := 5; i <= n; i++ {
		for j := 1; j < i; j++ {
			if dp[i] > j*dp[i-j] {
				dp[i] = dp[i]
			} else {
				dp[i] = j * dp[i-j]
			}
		}
	}
	return dp[n]
}
```

