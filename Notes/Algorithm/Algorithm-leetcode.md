# 算法-Leetcode

### 链表

#### [L2-中等] 两数相加

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例**

```
2 -> 4 -> 3
5 -> 6 -> 4
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807

输入：l1 = [0], l2 = [0]
输出：[0]

输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

- 每个链表中的节点数在范围 `[1, 100]` 内
- `0 <= Node.val <= 9`
- 题目数据保证列表表示的数字不含前导零

**题解**

Go：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    var head, cur *ListNode
    add := 0
    for l1 != nil || l2 != nil {
        n1, n2 := 0, 0
        if l1 != nil {
            n1 = l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            n2 = l2.Val
            l2 = l2.Next
        }

        sum := n1 + n2 + add
        sum, add = sum % 10, sum / 10

        if head == nil {
            head = &ListNode{Val: sum}
            cur = head
        } else {
            cur.Next = &ListNode{Val: sum}
            cur = cur.Next
        }
    }

    if add > 0 {
        cur.Next = &ListNode{Val: add}
    }

    return head
}
```

PHP：

```php
/**
 * Definition for a singly-linked list.
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val) { $this->val = $val; }
 * }
 */
class Solution {

    /**
     * @param ListNode $l1
     * @param ListNode $l2
     * @return ListNode
     */
    function addTwoNumbers($l1, $l2) {
        $add = 0;
        $list = new ListNode(0);
        $current = $list;
        while($l1 || $l2) {
            $x = 0;
            $y = 0;
            if ($l1 != null) {
                $x = $l1->val;
                $l1 = $l1->next;
            }
            if ($l2 != null) {
                $y = $l2->val;
                $l2 = $l2->next;
            }
            
            $val = ($x + $y + $add) % 10;
            $add = intval(($x + $y + $add) / 10);
            
            $new = new ListNode($val);
            $current->next = $new;
            $current = $current->next;
        }
        if ($add > 0) {
            $current->next = new ListNode($add);
        }
        return $list->next;
    }
}
```

#### [L19-中等] 删除链表的倒数第N个节点

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

给定的 n 保证是有效的。

**示例**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**题解**

Go:

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{0, head}
    fast, slow := dummy, dummy
	for i := 0; i <= n; i++ {
        fast = fast.Next
	}
    for fast != nil {
        fast = fast.Next
        slow = slow.Next
    }
    slow.Next = slow.Next.Next
	return dummy.Next
}
```

PHP:

```php
/**
 * Definition for a singly-linked list.
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val) { $this->val = $val; }
 * }
 */
class Solution {

    /**
     * @param ListNode $head
     * @param Integer $n
     * @return ListNode
     */
    function removeNthFromEnd($head, $n) {
        $dummy = new ListNode(0);
        $dummy->next = $head;
        $slow = $dummy;
        $first = $dummy;
        for ($i = 0; $i <= $n; $i++) {
            $first = $first->next;
        }
        while($first) {
            $slow = $slow->next;
            $first = $first->next;
        }
        $slow->next = $slow->next->next;
        return $dummy->next;
    }
}
```

#### [L21-简单] 合并两个有序链表

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

**题解**

**递归：**

Go：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 == nil{
        return l2
    }  
    if l2 == nil{
        return l1
    }
    var res *ListNode
    if l1.Val >= l2.Val{
        res = l2
        res.Next = mergeTwoLists(l1,l2.Next)
    }else{
        res = l1
        res.Next = mergeTwoLists(l1.Next,l2)
    }
    return res
}
```

**迭代：**

Go：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    if list1 == nil {
        return list2
    }
    if list2 == nil {
        return list1
    }

    dummy := &ListNode{0, nil}
    current := dummy
    for list1 != nil || list2 != nil {
        if list1 == nil {
            current.Next = list2
            break
        }
        if list2 == nil {
            current.Next = list1
            break
        }
        if list1.Val < list2.Val {
            current.Next = list1
            current = current.Next
            list1 = list1.Next
        } else {
            current.Next = list2
            current = current.Next
            list2 = list2.Next
        }
    }
    return dummy.Next
}
```

PHP：

```php
/**
 * Definition for a singly-linked list.
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val) { $this->val = $val; }
 * }
 */
class Solution {

    /**
     * @param ListNode $l1
     * @param ListNode $l2
     * @return ListNode
     */
    function mergeTwoLists($l1, $l2) {
        if (!$l1) return $l2;
        if (!$l2) return $l1;
        
        $dummyhead = new ListNode(0);
        $current = $dummyhead;
        while ($l1 || $l2) {
            if (!$l1) {
                $current->next = $l2;
                break;
            }
            if (!$l2) {
                $current->next = $l1;
                break;
            }
            if ($l1->val < $l2->val) {
                $current->next = $l1;
                $current = $current->next;
                $l1 = $l1->next;
            } else {
                $current->next = $l2;
                $current = $current->next;
                $l2 = $l2->next;
            }
        }
        return $dummyhead->next;
    }
}
```

有关php实现链表可以参考以下文章：https://www.cnblogs.com/sunshineliulu/p/7717301.html

#### [L24-中等] 两两交换链表中的节点

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例**

```
给定 1->2->3->4, 你应该返回 2->1->4->3.

输入：head = [1,2,3,4]
输出：[2,1,4,3]

输入：head = []
输出：[]

输入：head = [1]
输出：[1]
```

- 链表中节点的数目在范围 `[0, 100]` 内
- `0 <= Node.val <= 100`

**解题**

```
		  node1   node2   next
     dummy->1 ->    2  ->   3  -> 4
     dummy->2 -> 1 -> 3 -> 4
```

**代码实现**

递归：

```go
func swapPairs(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return head
	}
	newHead := head.Next
	head.Next = swapPairs(newHead.Next)
	newHead.Next = head
	return newHead
}
```

迭代：

Go：

```go
func swapPairs(head *ListNode) *ListNode {
    dummyHead := &ListNode{0, head}
    temp := dummyHead
    for temp.Next != nil && temp.Next.Next != nil {
        node1 := temp.Next
        node2 := temp.Next.Next

        temp.Next = node2
        node1.Next = node2.Next
        node2.Next = node1
        
        temp = node1
    }
    return dummyHead.Next
}
```

PHP：

```php
/**
 * Definition for a singly-linked list.
 * class ListNode {
 *     public $val = 0;
 *     public $next = null;
 *     function __construct($val) { $this->val = $val; }
 * }
 */
class Solution {

    /**
     * @param ListNode $head
     * @return ListNode
     */
    function swapPairs($head) {
        $dummyhead = new ListNode(0);
        $dummyhead->next = $head;
        $q = $dummyhead;
        while ($q->next && $q->next->next) {
            $node1 = $q->next;
            $node2 = $node1->next;
            $next = $node2->next;
            
            $node2->next = $node1;
            $node1->next = $next;
            $q->next = $node2;

            $q = $node1;
        }
        return $dummyhead->next;
    }
}
```

#### [L138-中等] 随机链表的复制

给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random` ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 **[深拷贝](https://baike.baidu.com/item/深拷贝/22785317?fr=aladdin)**。 深拷贝应该正好由 `n` 个 **全新** 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。**复制链表中的指针都不应指向原链表中的节点** 。

例如，如果原链表中有 `X` 和 `Y` 两个节点，其中 `X.random --> Y` 。那么在复制链表中对应的两个节点 `x` 和 `y` ，同样有 `x.random --> y` 。

返回复制链表的头节点。

用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：

- `val`：一个表示 `Node.val` 的整数。
- `random_index`：随机指针指向的节点索引（范围从 `0` 到 `n-1`）；如果不指向任何节点，则为 `null` 。

你的代码 **只** 接受原链表的头节点 `head` 作为传入参数。

**示例**

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]

输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

- `0 <= n <= 1000`
- `-104 <= Node.val <= 104`
- `Node.random` 为 `null` 或指向链表中的节点。

**题解**

```go
func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    // 创建一个哈希表，key是原节点，value是新节点
    hash := make(map[*Node]*Node, 0)
    p := head
    // 将原节点和新节点放入哈希表中
    for p != nil {
        newNode := &Node{Val: p.Val}
        hash[p] = newNode
        p = p.Next
    }
    p = head
    // 遍历原链表，设置新节点的next和random
    for p != nil {
        // 获取新节点
        newNode := hash[p]
        // 原节点的下一个节点存在，则更新新节点的下一个节点
        if p.Next != nil {
            newNode.Next = hash[p.Next]
        }
        // 原节点的随机节点存在，则更新新节点的随机节点
        if p.Random != nil {
            newNode.Random = hash[p.Random]
        }
        p = p.Next
    }
    // 返回头节点，即原节点对应的 value 值
    return hash[head]
}
```

#### [L141-简单] 环形链表

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

**示例**

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

- 链表中节点的数目范围是 `[0, 104]`
- `-105 <= Node.val <= 105`
- `pos` 为 `-1` 或者链表中的一个 **有效索引** 。

**题解**

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func hasCycle(head *ListNode) bool {
    if head == nil {
        return false
    }

    hash := map[*ListNode]struct{}{}
    for head != nil {
        if _, ok := hash[head]; ok {
            return true
        }
        hash[head] = struct{}{}
        head = head.Next
    }
    return false
}
```

#### [L142-中等] 环形链表 II

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。

**示例**

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。

输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。

输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

**题解**

```go
func detectCycle(head *ListNode) *ListNode {
    hash := map[*ListNode]struct{}{}
    i := 0
    for head != nil {
        if _, ok := hash[head]; ok {
            return head
        }
        i++
        hash[head] = struct{}{}
        head = head.Next
    }
    return nil
}
```

#### [L146-中等] LRU 缓存

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

**示例**

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`
- 最多调用 `2 * 105` 次 `get` 和 `put`

**题解**

双向链表+hash

```go
// DLinkedNode 定义双向链表结构体
type DLinkedNode struct {
	key, value int          // 键值对
	prev, next *DLinkedNode // 指向前驱、后继节点的指针
}

// newDLinkedNode 初始化链表
func newDLinkedNode(key, value int) *DLinkedNode {
	return &DLinkedNode{
		key:   key,
		value: value,
	}
}

// LRUCache 缓存结构
type LRUCache struct {
	size       int                  // 当前已用容量大小
	capacity   int                  // 容量
	cache      map[int]*DLinkedNode // 哈希表，存储缓存数据的键映射到双向链表的位置
	head, tail *DLinkedNode         // 头部和尾部节点
}

// Constructor 构造函数
func Constructor(capacity int) LRUCache {
	l := LRUCache{
		cache:    map[int]*DLinkedNode{},
		head:     newDLinkedNode(0, 0),
		tail:     newDLinkedNode(0, 0),
		capacity: capacity,
	}
	l.head.next = l.tail
	l.tail.prev = l.head
	return l
}

// Get 获取缓存值
func (t *LRUCache) Get(key int) int {
	// 不存在，则返回-1
	if _, ok := t.cache[key]; !ok {
		return -1
	}
	// 获取该值在双向链表中的位置
	node := t.cache[key]
	// 先删除该节点，再添加至链表头部（使访问过的节点处于最新状态）
	t.moveToHead(node)
	// 返回节点的值
	return node.value
}

// Put 插入或更新缓存值
func (t *LRUCache) Put(key int, value int) {
	if node, ok := t.cache[key]; ok { // 已存在，则更新
		node.value = value
		// 先删除该节点，再添加至链表头部（使访问过的节点处于最新状态）
		t.moveToHead(node)
	} else { // 不存在
		// 以该 key value 初始化一个节点
		node = newDLinkedNode(key, value)
		// 加入hash表映射
		t.cache[key] = node
		// 添加该节点至头部
		t.addToHead(node)
		t.size++
		// 若加入该缓存后容量超过最大限制，则删除最后一个缓存
		if t.size > t.capacity {
			rmNode := t.removeTail()    // 删除链表最后一个节点
			delete(t.cache, rmNode.key) // 删除该节点在hash表中的映射关系
			t.size--
		}
	}
}

// 添加节点至头部
func (t *LRUCache) addToHead(node *DLinkedNode) {
	node.prev = t.head
	node.next = t.head.next
	t.head.next.prev = node
	t.head.next = node
}

// 删除节点
func (t *LRUCache) removeNode(node *DLinkedNode) {
	node.prev.next = node.next
	node.next.prev = node.prev
}

// 删除节点，并将该节点添加至头部
func (t *LRUCache) moveToHead(node *DLinkedNode) {
	t.removeNode(node)
	t.addToHead(node)
}

// 删除链表最后一个节点
func (t *LRUCache) removeTail() *DLinkedNode {
	node := t.tail.prev
	t.removeNode(node)
	return node
}
```

#### [L148-中等] 排序链表

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

**示例**

```
输入：head = [4,2,1,3]
输出：[1,2,3,4]

输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]

输入：head = []
输出：[]
```

- 链表中节点的数目在范围 `[0, 5 * 104]` 内
- `-105 <= Node.val <= 105`

**题解**

使用归并排序思路，只是这里是链表，链表的切分和合并需要注意

链表切分：

1. 通过快慢指针找出链表中点
2. 在中点处切断链表，一分为二（通过将中等位置链表的 next 指针置为 nil 来切断）

链表合并：

1. 需要创建一个哑节点作为新链表的头
2. 依次对比左右链表第一个元素的大小，将小的先加入结果链表，这里思路和数组合并一致

```go
func sortList(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return head
	}
	// 定义快慢指针
	slow, fast := head, head.Next
	// 慢指针每次走1步，快指针每次走2步，当快指针fast走到最后的时候，慢指针的位置就是中点
	// 当链表为偶数是，slow指向中间前一个元素
	for fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next
	}
	// 暂存
	tmp := slow.Next
	// 切断链表
	slow.Next = nil
	// 递归左右链表，直到切分每个链表剩一个节点终止
	left := sortList(head)
	right := sortList(tmp)

	// 返回合并的结果
	return merge(left, right)
}

// 合并左右链表
func merge(left, right *ListNode) *ListNode {
    // 创建一个哑节点作为新链表的头
	dummy := &ListNode{}
	curr := dummy
	for left != nil && right != nil {
		if left.Val < right.Val {
			curr.Next = left
			left = left.Next
		} else {
			curr.Next = right
			right = right.Next
		}
		curr = curr.Next
	}
	// 链接剩余的部分
	if left != nil {
		curr.Next = left
	}
	if right != nil {
		curr.Next = right
	}
	// 返回新链表的头（跳过哑节点）
	return dummy.Next
}
```

#### [L160-简单] 相交链表

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 

**题解**

哈希表，先遍历链表A，存入hash，再遍历链表B，如果当前节点在hash表中，且后面的节点都在，则相交

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    hash := map[*ListNode]int{}
    tmpA, tmpB := headA, headB
    for tmpA != nil {
        hash[tmpA] = tmpA.Val
        tmpA = tmpA.Next
    }
    for tmpB != nil {
        if hash[tmpB] == tmpB.Val {
            return tmpB
        }
        tmpB = tmpB.Next
    }
    return nil
}
```

双指针

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }
    pa, pb := headA, headB
    for pa != pb {
        if pa == nil {
            pa = headB
        } else {
            pa = pa.Next
        }
        if pb == nil {
            pb = headA
        } else {
            pb = pb.Next
        }
    }
    return pa
}
```

#### [L206-简单] 反转链表

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

**示例**

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]

输入：head = [1,2]
输出：[2,1]

输入：head = []
输出：[]
```

**题解**

先遍历链表，存在数组中，再遍历数组，构建新的链表

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    if head == nil {
		return head
	}
    arr := make([]int, 0)
    for head != nil {
        arr = append([]int{head.Val}, arr...)
        head = head.Next
    }
    newHead := &ListNode{Val: arr[0], Next: nil}
    tmpHead := newHead
    for i := 1; i < len(arr); i++ {
        tmpHead.Next = &ListNode{arr[i], nil}
        tmpHead = tmpHead.Next
    }
    return newHead
}
```

迭代：在遍历链表时，将当前节点的 next 指针改为指向前一个节点

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode
    curr := head
    for curr != nil {
        next := curr.Next
        curr.Next = prev
        prev = curr
        curr = next
    }
    return prev
}
```

#### [L234-简单] 回文链表

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

**示例**

```
输入：head = [1,2,2,1]
输出：true

输入：head = [1,2]
输出：false
```

**题解**

先遍历链表存入数组，再遍历数组进行判断

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func isPalindrome(head *ListNode) bool {
    if head == nil {
        return false
    }
    tmp := make([]int, 0)
    for head != nil {
        tmp = append(tmp, head.Val)
        head = head.Next
    }
    n := len(tmp)
    for i := 0; i < n/2; i++ {
        if tmp[i] != tmp[n-1-i] {
            return false
        }
    }
    return true
}
```

### 二叉树

#### [L94-简单] 二叉树的中序遍历

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

**示例**

```
输入：root = [1,null,2,3]
输出：[1,3,2]

输入：root = []
输出：[]

输入：root = [1]
输出：[1]
```

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

**题解**

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func inorderTraversal(root *TreeNode) (res []int) {
    var inorder func(node *TreeNode)
    inorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        inorder(node.Left)
        res = append(res, node.Val)
        inorder(node.Right)
    }
    inorder(root)
    return
}
```

#### [L101-简单] 对称二叉树

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

**示例**

```
输入：root = [1,2,2,3,4,4,3]
输出：true

输入：root = [1,2,2,null,3,null,3]
输出：false
```

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

**题解**

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSymmetric(root *TreeNode) bool {
    return check(root, root)
}

func check(p, q *TreeNode) bool {
    if p == nil && q == nil {
        return true
    }
    if p == nil || q == nil {
        return false
    }
    return p.Val == q.Val && check(p.Left, q.Right) && check(p.Right, q.Left)
}
```

#### [L102-简单] 二叉树的层次遍历

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

**示例**

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]

输入：root = [1]
输出：[[1]]

输入：root = []
输出：[]
```

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

**题解**

```go
func levelOrder(root *TreeNode) [][]int {
	ret := make([][]int, 0)
	if root == nil {
		return ret
	}
	q := []*TreeNode{root}
	for len(q) > 0 {
		// 收集当前层的所有值
		level := make([]int, 0)
		p := make([]*TreeNode, 0)
		for _, node := range q {
			level = append(level, node.Val)
			if node.Left != nil {
				p = append(p, node.Left)
			}
			if node.Right != nil {
				p = append(p, node.Right)
			}
		}
		// 将收集到的值添加到结果集中
		ret = append(ret, level)
		// 更新队列为下一层的节点
		q = p
	}
	return ret
}
```

#### [L104-简单] 二叉树的最大深度

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

**示例**

```
输入：root = [3,9,20,null,null,15,7]
输出：3

输入：root = [1,null,2]
输出：2
```

- 树中节点的数量在 `[0, 104]` 区间内。
- `-100 <= Node.val <= 100`

**题解**

> 深度优先搜索

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    return max(maxDepth(root.Left), maxDepth(root.Right)) + 1
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### [L108-简单] 将有序数组转化为二叉搜索树

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 平衡 二叉搜索树。

**示例**

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案

输入：nums = [1,3]
输出：[3,1]
解释：[1,null,3] 和 [3,1] 都是高度平衡二叉搜索树。
```

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 按 **严格递增** 顺序排列

**题解**

```go
func sortedArrayToBST(nums []int) *TreeNode {
	return helper(nums, 0, len(nums)-1)
}

func helper(nums []int, left, right int) *TreeNode {
	if left > right {
		return nil
	}
	mid := (left + right) / 2
	root := &TreeNode{Val: nums[mid]}
	root.Left = helper(nums, left, mid-1)
	root.Right = helper(nums, mid+1, right)
	return root
}
```

#### [L144-简单] 二叉树的前序遍历

给你一棵二叉树的根节点 `root` ，返回其节点值的 **后前遍历** 。

**示例**

```
输入：root = [1,null,2,3]
输出：[1,2,3]

输入：root = []
输出：[]

输入：root = [1]
输出：[1]
```

- 树中节点的数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

**题解**

```go
func preorderTraversal(root *TreeNode) (res []int) {
	var preorder func(node *TreeNode)
	preorder = func(node *TreeNode) {
		if node == nil {
			return
		}
		res = append(res, node.Val)
		preorder(node.Left)
		preorder(node.Right)
	}
	preorder(root)
	return
}
```

#### [L145-简单] 二叉树的后序遍历

给你一棵二叉树的根节点 `root` ，返回其节点值的 **后序遍历** 。

**示例**

```
输入：root = [1,null,2,3]
输出：[3,2,1]

输入：root = []
输出：[]

输入：root = [1]
输出：[1]
```

- 树中节点的数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

**题解**

```go
func postorderTraversal(root *TreeNode) (res []int) {
	var postorder func(node *TreeNode)
	postorder = func(node *TreeNode) {
		if node == nil {
			return
		}
		postorder(node.Left)
		postorder(node.Right)
		res = append(res, node.Val)
	}
	postorder(root)
	return
}
```

#### [L226-简单] 翻转二叉树

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

**示例**

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]

输入：root = [2,1,3]
输出：[2,3,1]

输入：root = []
输出：[]
```

- 树中节点数目范围在 `[0, 100]` 内
- `-100 <= Node.val <= 100`

**题解**

> 递归
>

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
		return nil
	}
	// 交换左右节点位置
	tmp := root.Left
	root.Left = root.Right
	root.Right = tmp
	// 递归
	invertTree(root.Left)
	invertTree(root.Right)
	return root
}
```

#### [L543-简单] 二叉树的直径

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

**示例**

```
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。

输入：root = [1,2]
输出：1
```

- 树中节点数目在范围 `[1, 104]` 内
- `-100 <= Node.val <= 100`

**题解**

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
var result int
func diameterOfBinaryTree(root *TreeNode) int {
    result = 1
    depth(root)
    return result - 1
}
    
func depth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    l := depth(root.Left)
    r := depth(root.Right)
    if result < l + r + 1 {
        result = l + r + 1
    }
    if l > r {
        return l + 1
    } else {
        return r + 1
    }
}
```

#### [L617-简单] 合并二叉树

给你两棵二叉树： `root1` 和 `root2` 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，**不为** null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意:** 合并过程必须从两个树的根节点开始。

**示例**

```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]

输入：root1 = [1], root2 = [1,2]
输出：[2,2]
```

- 两棵树中的节点数目在范围 `[0, 2000]` 内
- `-104 <= Node.val <= 104`

**题解**

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func mergeTrees(root1 *TreeNode, root2 *TreeNode) *TreeNode {
    if root1 == nil {
        return root2
    }
    if root2 == nil {
        return root1
    }
    root1.Val += root2.Val
    root1.Left = mergeTrees(root1.Left, root2.Left)
    root1.Right = mergeTrees(root1.Right, root2.Right)
    return root1
}
```

#### [L98-中等] 验证二叉搜索树

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含小于当前节点的数。
- 节点的右子树只包含 **大于**当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例**

```
输入：root = [2,1,3]
输出：true

输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

- 树中节点数目范围在`[1, 104]` 内
- `-231 <= Node.val <= 231 - 1`

**题解**

二叉搜索树中序遍历结果为递增序列，利用这一特性进行验证

```go
func isValidBST(root *TreeNode) bool {
    var inorder func(node *TreeNode)
    // 根据题意，节点最小值为-2的32次方
    tmp := -int64(math.Pow(-2, 32))
    res := true
    inorder = func(node *TreeNode) {
        // 已知结果，直接返回
        if res == false {
            return
        }
        if node == nil {
            return
        }
        inorder(node.Left)
        // 当前节点值必需大于前一个节点值，否则为非二叉搜索树
        if int64(node.Val) <= tmp {
            res = false
            return 
        }
        tmp = int64(node.Val)
        inorder(node.Right)
    }
    inorder(root)
    return res
}
```

#### [L105-中等] 从前序与中序遍历序列构造二叉树

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

**示例**

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]

输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` 和 `inorder` 均 **无重复** 元素
- `inorder` 均出现在 `preorder`
- `preorder` **保证** 为二叉树的前序遍历序列
- `inorder` **保证** 为二叉树的中序遍历序列

**题解**

前序：`[ 根节点, [左子树的前序遍历结果], [右子树的前序遍历结果] ]`

中序：`[ [左子树的中序遍历结果], 根节点, [右子树的中序遍历结果] ]`

前序遍历第一个元素为二叉树的根节点

在中序遍历中找到根节点，则可以知道左子树和右子树的长度

因为前序和中序的左右子树结果长度一致，只是顺序不同，因此可以根据中序的左右子树长度来映射前序中左右子树的长度

安装前序遍历进行递归，还原二叉树

```go
// 递归
func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) == 0 {
		return nil
	}
	// 已知前序遍历第一个元素为根节点
	root := &TreeNode{Val: preorder[0]}
	// 在中序遍历列表中找到根节点位置
	i := 0
	for ; i < len(inorder); i++ {
		if inorder[i] == preorder[0] {
			break
		}
	}
	// 左子树的长度
	l := len(inorder[:i])
	root.Left = buildTree(preorder[1:l+1], inorder[:i])
	root.Right = buildTree(preorder[l+1:], inorder[i+1:])
	return root
}
```

#### [L114-中等] 二叉树展开为链表

给你二叉树的根结点 `root` ，请你将它展开为一个单链表：

- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
- 展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/先序遍历/6442839?fr=aladdin) 顺序相同。

**示例**

```
输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]

输入：root = []
输出：[]

输入：root = [0]
输出：[0]
```

- 树中结点数在范围 `[0, 2000]` 内
- `-100 <= Node.val <= 100`

**题解**

```go
func flatten(root *TreeNode) {
	// 前序遍历二叉树，存储节点
	list := make([]*TreeNode, 0)
	var preorder func(node *TreeNode)
	preorder = func(node *TreeNode) {
		if node == nil {
			return
		}
		list = append(list, node)
		preorder(node.Left)
		preorder(node.Right)
	}
	preorder(root)
	// 遍历节点列表，修改二叉树左右节点信息
	for i := 1; i < len(list); i++ {
		prev, curr := list[i-1], list[i]
		prev.Left, prev.Right = nil, curr
	}
}
```

#### [L199-中等] 二叉树的右视图

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

**示例**

```
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]

输入: [1,null,3]
输出: [1,3]

输入: []
输出: []
```

- 二叉树的节点个数的范围是 `[0,100]`
- `-100 <= Node.val <= 100` 

**题解**

二叉树的层次遍历，取每一层最后一个元素

```
func rightSideView(root *TreeNode) []int {
	ans := make([]int, 0)
	if root == nil {
		return ans
	}
	// 层次遍历
	q := []*TreeNode{root}
	for len(q) > 0 {
		// 收集当前层的所有值
		level := make([]int, 0)
		p := make([]*TreeNode, 0)
		for _, node := range q {
			level = append(level, node.Val)
			if node.Left != nil {
				p = append(p, node.Left)
			}
			if node.Right != nil {
				p = append(p, node.Right)
			}
		}
		// 将收集到的值添加到结果集中
		ans = append(ans, level[len(level)-1])
		// 更新队列为下一层的节点
		q = p
	}
	return ans
}
```

#### [230-中等] 二叉搜索树中第K小的元素

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 小的元素（从 1 开始计数）。

**示例**

```
输入：root = [3,1,4,null,2], k = 1
输出：1

输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3
```

- 树中的节点数为 `n` 。
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

**题解**

二叉搜索树中序遍历结果为递增序列，利用这一特性，遍历过程 `k--`，当k为0时则就是结果

```go
func kthSmallest(root *TreeNode, k int) int {
    var inorder func(node *TreeNode)
    ans := 0
    inorder = func(node *TreeNode) {
        if ans != 0 {
            return
        }
        if node == nil {
            return
        }
        inorder(node.Left)
        k--
        if k == 0 {
            ans = node.Val
            return
        }
        inorder(node.Right)
    }
    inorder(root)
    return ans
}
```

#### [L236-中等] 二叉树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

**示例**

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。

输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。

输入：root = [1,2], p = 1, q = 2
输出：1
```

**题解**

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    // 当越过叶节点，则直接返回 null
    if root == nil {
        return nil
    }
    // 当 root 等于 p,q ，则直接返回 root
    if root == p || root == q {
        return root
    }
    // 分别递归左右子树
    left := lowestCommonAncestor(root.Left, p, q)
    right := lowestCommonAncestor(root.Right, p, q)
    if left == nil && right == nil {
        return nil
    }
    if left == nil {
        return right
    }
    if right == nil {
        return left
    }
    return root
}
```

#### [L437-中等] 路径总和 III

给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

**示例**

```
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条

输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：3
```

- 二叉树的节点个数的范围是 `[0,1000]`
- `-109 <= Node.val <= 109` 
- `-1000 <= targetSum <= 1000` 

**题解**

```go
func rootSum(root *TreeNode, targetSum int) (res int) {
    if root == nil {
        return
    }
    val := root.Val
    if val == targetSum {
        res++
    }
    res += rootSum(root.Left, targetSum-val)
    res += rootSum(root.Right, targetSum-val)
    return
}

func pathSum(root *TreeNode, targetSum int) int {
    if root == nil {
        return 0
    }
    res := rootSum(root, targetSum)
    res += pathSum(root.Left, targetSum)
    res += pathSum(root.Right, targetSum)
    return res
}
```

### 图

#### [L200-中等] 岛屿数量

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例**

```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1

输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` 的值为 `'0'` 或 `'1'`

**题解**

岛屿问题的深度优先遍历，本题传入的是char类型，需要注意

```go
func dfsIslands(grid [][]byte, r, c int) {
	// 超出边界，返回
	if r < 0 || len(grid) <= r || c < 0 || len(grid[0]) <= c {
		return
	}
	// 如果这个格子不是岛屿，直接返回
	if grid[r][c] != '1' {
		return
	}
	// 将遍历过的格子标记为2
	grid[r][c] = '2'
	// 访问上、下、左、右相邻节点
	dfsIslands(grid, r-1, c)
	dfsIslands(grid, r+1, c)
	dfsIslands(grid, r, c-1)
	dfsIslands(grid, r, c+1)
}

func numIslands(grid [][]byte) int {
	ans := 0
	for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[0]); j++ {
			if grid[i][j] == '1' {
				ans++
				dfsIslands(grid, i, j)
			}
		}
	}
	return ans
}
```

#### [L463-中等] 岛屿的周长

给定一个 `row x col` 的二维网格地图 `grid` ，其中：`grid[i][j] = 1` 表示陆地， `grid[i][j] = 0` 表示水域。

网格中的格子 **水平和垂直** 方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

**示例**

```
输入：grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
输出：16
解释：它的周长是上面图片中的 16 个黄色的边

输入：grid = [[1]]
输出：4

输入：grid = [[1,0]]
输出：4
```

- `row == grid.length`
- `col == grid[i].length`
- `1 <= row, col <= 100`
- `grid[i][j]` 为 `0` 或 `1`

**题解**

```go
func dfsPerimeter(grid [][]int, r, c int) int {
    // 超出边界，刚好是边长，返回1
    if r < 0 || len(grid) <= r || c < 0 || len(grid[0]) <= c {
        return 1
    }
    // 如果这个格子是海洋格子，也是边长，返回1
    if grid[r][c] == 0 {
        return 1
    }
    // 如果这个格子是已经遍历过的岛屿，返回0
    if grid[r][c] == 2 {
        return 0
    }
    // 将遍历过的格子标记为2
    grid[r][c] = 2
    // 访问上、下、左、右相邻节点
    return dfsPerimeter(grid, r - 1, c) + dfsPerimeter(grid, r + 1, c) + dfsPerimeter(grid, r, c - 1) + dfsPerimeter(grid, r, c + 1)
}

func islandPerimeter(grid [][]int) int {
    for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[0]); j++ {
			if grid[i][j] == 1 {
				return dfsPerimeter(grid, i, j)
			}
		}
	}
    return 0
}
```

#### [L695-中等] 岛屿的最大面积

给你一个大小为 `m x n` 的二进制矩阵 `grid` 。

**岛屿** 是由一些相邻的 `1` (代表土地) 构成的组合，这里的「相邻」要求两个 `1` 必须在 **水平或者竖直的四个方向上** 相邻。你可以假设 `grid` 的四个边缘都被 `0`（代表水）包围着。

岛屿的面积是岛上值为 `1` 的单元格的数目。

计算并返回 `grid` 中最大的岛屿面积。如果没有岛屿，则返回面积为 `0` 。

**示例**

```
输入：grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
输出：6
解释：答案不应该是 11 ，因为岛屿只能包含水平或垂直这四个方向上的 1 。

输入：grid = [[0,0,0,0,0,0,0,0]]
输出：0
```

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` 为 `0` 或 `1`

**题解**

求出每一个岛屿的面积（使用DFS），再取最大值

```go
// 判断格子是否在边界内
func inArea(grid [][]int, r, c int) bool {
    return 0 <= r && r < len(grid) && 0 <= c && c < len(grid[0])
}

// 深度优先搜索求面积
func dfs(grid [][]int, r, c int) int {
    // 超出边界，返回
    if !inArea(grid, r, c) {
        return 0
    }
    // 如果这个格子不是岛屿，直接返回
    if grid[r][c] != 1 {
        return 0
    }
    // 将遍历过的格子标记为2
    grid[r][c] = 2
    // 访问上、下、左、右相邻节点
    return 1 + dfs(grid, r - 1, c) + dfs(grid, r + 1, c) + dfs(grid, r, c - 1) + dfs(grid, r, c + 1)
}

func maxAreaOfIsland(grid [][]int) int {
    max := 0
    for i := 0; i < len(grid); i++ {
        for j := 0; j < len(grid[0]); j++ {
            if grid[i][j] == 1 {
                area := dfs(grid, i, j)
                if area > max {
                    max = area
                }
            }
        }
    }
    return max
}
```

#### [L994-中等] 腐烂的橘子

在给定的 `m x n` 网格 `grid` 中，每个单元格可以有以下三个值之一：

- 值 `0` 代表空单元格；
- 值 `1` 代表新鲜橘子；
- 值 `2` 代表腐烂的橘子。

每分钟，腐烂的橘子 **周围 4 个方向上相邻** 的新鲜橘子都会腐烂。

返回 *直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1`* 。

**示例**

```
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]
输出：4

输入：grid = [[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个方向上。

输入：grid = [[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```

**题解**

```go
func orangesRotting(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    // 腐烂橘子坐标
    queue := make([][]int, 0)
    // 新鲜橘子的数量
    count := 0
    // 遍历网格，获取新鲜橘子数量和腐烂橘子坐标集合
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 1 {
                count++
            } else if grid[i][j] == 2 {
                queue = append(queue, []int{i, j})
            }
        }
    }
    // 腐烂遍历次数 == 分钟
    round := 0
    for count > 0 && len(queue) > 0 {
        round++
        // 获取当前队列长度
        size := len(queue)
        // 遍历当前层，将新鲜橘子更新为腐烂橘子，并更新新一轮腐烂橘子的队列
        for i := 0; i < size; i++ {// 使用size来避免在迭代中改变queue的长度
            orange := queue[0]
            queue = queue[1:]
            r, c := orange[0], orange[1]
            // 分别在腐烂橘子的上、下、左、右进行感染
            for r-1 >= 0 && grid[r-1][c] == 1 {// 上
                grid[r-1][c] = 2
                count--
                queue = append(queue, []int{r-1, c})
            }
            if r+1 < m && grid[r+1][c] == 1 {// 下
                grid[r+1][c] = 2
                count--
                queue = append(queue, []int{r+1, c})
            }
            if c-1 >= 0 && grid[r][c-1] == 1 {// 左
                grid[r][c-1] = 2
                count--
                queue = append(queue, []int{r, c-1})
            }
            if c+1 < n && grid[r][c+1] == 1 {// 右
                grid[r][c+1] = 2
                count--
                queue = append(queue, []int{r, c+1})
            }
        }
    }
    if count > 0 {
        return -1
    }
    return round
}
```

#### [L207-中等] 课程表

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi` 。

- 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。

请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

**示例**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。

输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
```

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `prerequisites[i]` 中的所有课程对 **互不相同**

**题解**

深度优先搜索

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    // 结果集
	res := make([]int, 0)
	// 遍历过的节点，存入栈
	visited := make([]int, numCourses)
	// 邻接表
	edges := make([][]int, numCourses)
	// 是否存在拓扑排序（如果一个有向图包含环，则不存在）
	valid := true
	// 深度优先遍历
	var dfs func(u int)
	dfs = func(u int) {
		// 标记为搜索中：1
		visited[u] = 1
		for _, v := range edges[u] {
			if visited[v] == 0 { // 未搜索，则开始搜索
				dfs(v)
				if !valid {
					return
				}
			} else if visited[v] == 1 { // 搜索中，说明找到了环
				valid = false
				return
			}
		}
		// 搜索完成，修改为已完成状态
		visited[u] = 2
		// 记录结果
		res = append(res, u)
	}
	// 构建邻接表
	// i 对应于一个节点（即课程），edges[i] 则是一个包含所有以课程 i 为先修课程的课程列表。
	for _, info := range prerequisites {
		edges[info[1]] = append(edges[info[1]], info[0])
	}
	// 遍历，对未搜索的节点进行深度优先搜索
	for i := 0; i < numCourses && valid; i++ {
		if visited[i] == 0 {
			dfs(i)
		}
	}
	return valid
}
```

#### [L210-中等] 课程表II

现在你总共有 `numCourses` 门课需要选，记为 `0` 到 `numCourses - 1`。给你一个数组 `prerequisites` ，其中 `prerequisites[i] = [ai, bi]` ，表示在选修课程 `ai` 前 **必须** 先选修 `bi` 。

- 例如，想要学习课程 `0` ，你需要先完成课程 `1` ，我们用一个匹配来表示：`[0,1]` 。

返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回 **任意一种** 就可以了。如果不可能完成所有课程，返回 **一个空数组** 。

**示例**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：[0,1]
解释：总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。

输入：numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
输出：[0,2,1,3]
解释：总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。

输入：numCourses = 1, prerequisites = []
输出：[0]
```

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `ai != bi`
- 所有`[ai, bi]` **互不相同**

**题解**

深度优先搜索（dfs）

```go
func findOrder(numCourses int, prerequisites [][]int) []int {
	// 结果集
	res := make([]int, 0)
	// 遍历过的节点，存入栈
	visited := make([]int, numCourses)
	// 邻接表
	edges := make([][]int, numCourses)
	// 是否存在拓扑排序（如果一个有向图包含环，则不存在）
	valid := true
	// 深度优先遍历
	var dfs func(u int)
	dfs = func(u int) {
		// 标记为搜索中：1
		visited[u] = 1
		for _, v := range edges[u] {
			if visited[v] == 0 { // 未搜索，则开始搜索
				dfs(v)
				if !valid {
					return
				}
			} else if visited[v] == 1 { // 搜索中，说明找到了环
				valid = false
				return
			}
		}
		// 搜索完成，修改为已完成状态
		visited[u] = 2
		// 记录结果
		res = append(res, u)
	}
	// 构建邻接表
	// i 对应于一个节点（即课程），edges[i] 则是一个包含所有以课程 i 为先修课程的课程列表。
	for _, info := range prerequisites {
		edges[info[1]] = append(edges[info[1]], info[0])
	}
	// 遍历，对未搜索的节点进行深度优先搜索
	for i := 0; i < numCourses && valid; i++ {
		if visited[i] == 0 {
			dfs(i)
		}
	}
	// 非拓扑排序，则不存在，返回空数组
	if !valid {
		return []int{}
	}
	// 反转结果
	for i := 0; i < len(res)/2; i++ {
		res[i], res[numCourses-i-1] = res[numCourses-i-1], res[i]
	}
	return res
}
```

广度优先搜索（bfs）

```go
// 广度优先搜索（bfs）
func findOrder(numCourses int, prerequisites [][]int) []int {
	// 邻接表
	edges := make([][]int, numCourses)
	// 每门课程的入度
	indeg := make([]int, numCourses)
	// 结果集
	res := make([]int, 0)
	// 构建邻接表
	for _, info := range prerequisites {
		// edges[i] 表示课程 i 的所有后继课程（即依赖于课程 i 的课程）。
		edges[info[1]] = append(edges[info[1]], info[0])
		// 获取每门课程的入度（有多少课程依赖它）
		indeg[info[0]]++
	}
	// 找出所有入度为0的节点
	q := []int{}
	for i := 0; i < numCourses; i++ {
		if indeg[i] == 0 {
			q = append(q, i)
		}
	}
	// 遍历入度为0的节点
	for len(q) > 0 {
		u := q[0]
		q = q[1:]
		// 入度为0，则说明已不依赖其他节点，加入结果
		res = append(res, u)
		// 删除节点（删除节点所有度）
		for _, v := range edges[u] {
			indeg[v]--
			if indeg[v] == 0 {
				q = append(q, v)
			}
		}
	}
	if len(res) != numCourses {
		return []int{}
	}
	return res
}
```

#### [L208-中等] 实现 Trie (前缀树)

**[Trie](https://baike.baidu.com/item/字典树/9825209?fr=aladdin)**（发音类似 "try"）或者说 **前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：

- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 

**示例**

```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

- `1 <= word.length, prefix.length <= 2000`
- `word` 和 `prefix` 仅由小写英文字母组成
- `insert`、`search` 和 `startsWith` 调用次数 **总计** 不超过 `3 * 104` 次

**题解**

```go
// Trie 前缀树结构体
type Trie struct {
    // 指向子节点的指针数组
    // children[0]对应小写字母a，children[1]对应小写字母b，…，children[25]对应小写字母z
    children [26]*Trie
    // 该节点是否为字符串结尾
    isEnd bool
}

// Constructor 构造函数
func Constructor() Trie {
    return Trie{}
}

// Insert 插入单词
func (this *Trie) Insert(word string)  {
    node := this
    for _, ch := range word {
        // 减去小写字母a的ASCII值（97）：a->0，b->1，...，z->25
        ch -= 'a'
        // 子节点不存在，则创建一个新的节点
        if node.children[ch] == nil {
            node.children[ch] = &Trie{}
        }
        // 沿着指针移动到子节点，继续处理下一个字符
        node = node.children[ch]
    }
    // 处理到最后一个字符，标记为结束位置
    node.isEnd = true
}

// Search 查找单词
func (this *Trie) Search(word string) bool {
    node := this.SearchPrefix(word)
    return node != nil && node.isEnd
}

// StartsWith 单词是否是这个前缀
func (this *Trie) StartsWith(prefix string) bool {
    return this.SearchPrefix(prefix) != nil
}

// SearchPrefix 查找前缀
func (this *Trie) SearchPrefix(prefix string) *Trie {
    node := this
    // 遍历前缀所有单词，依次查找
    for _, ch := range prefix {
        ch -= 'a'
        // 有一个单词不存在，则直接返回 nil
        if node.children[ch] == nil {
            return nil
        }
        node = node.children[ch]
    }
    return node
}


/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */
```

### 栈

#### [L20-简单] 有效的括号

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例**

```
输入: "()"
输出: true

输入: "()[]{}"
输出: true

输入: "(]"
输出: false

输入: "([)]"
输出: false

输入: "{[]}"
输出: true
```

**题解**

如果属于左侧括号，则向数组end中插入对应的右侧括号；如果属于右侧括号则查找end数组中是否有一样的，如果有则删去
类似于用栈实现

Go：

```go

func isValid(s string) bool {
	bracketsMap := map[byte]byte{
		'(': ')',
		'{': '}',
		'[': ']',
	}
	stack := make([]byte, 0)
	for i := 0; i < len(s); i++ {
		if bracketsMap[s[i]] > 0 {
			// 左括号，将对应右括号入栈
			stack = append(stack, bracketsMap[s[i]])
		} else { // 右括号，判断栈中是否有匹配
			// 无匹配，返回 false
			if len(stack) == 0 || stack[len(stack)-1] != s[i] {
				return false
			}
			// 有匹配，进行出栈
			stack = stack[:len(stack)-1]
		}
	}
	return len(stack) == 0
}
```

PHP：

```php
class Solution {

    /**
     * @param String $s
     * @return Boolean
     */
    function isValid($s) {
        if (empty($s)) {
            return true;
        }
        $arr = array(
            "(" => ")",
            "{" => "}",
            "[" => "]",
        );
        $end = [];
        for ($i = 0; $i < strlen($s); $i++) {
            if (isset($arr[$s[$i]])) {
                $end[] = $arr[$s[$i]];
            } elseif (end($end) == $s[$i]){
                array_pop($end);
            } else {
                return false;
            }
        }
        return count($end) === 0;
    }
}
```

#### [L155-中等] 最小栈

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

**示例**

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

- `-231 <= val <= 231 - 1`
- `pop`、`top` 和 `getMin` 操作总是在 **非空栈** 上调用
- `push`, `pop`, `top`, and `getMin`最多被调用 `3 * 104` 次

**题解**

```go
type MinStack struct {
	stack    []int
	minStack []int
}

func Constructor() MinStack {
	return MinStack{
		stack:    []int{},
		minStack: []int{math.MaxInt64},
	}
}

func (t *MinStack) Push(val int) {
	t.stack = append(t.stack, val)
	top := t.minStack[len(t.minStack)-1]
	if top < val {
		t.minStack = append(t.minStack, top)
	} else {
		t.minStack = append(t.minStack, val)
	}
}

func (t *MinStack) Pop() {
	t.stack = t.stack[:len(t.stack)-1]
	t.minStack = t.minStack[:len(t.minStack)-1]
}

func (t *MinStack) Top() int {
	return t.stack[len(t.stack)-1]
}

func (t *MinStack) GetMin() int {
	return t.minStack[len(t.minStack)-1]
}
```

#### [L349-中等] 字符串解码

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

**示例**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"

输入：s = "3[a2[c]]"
输出："accaccacc"

输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"

输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

- `1 <= s.length <= 30`
- `s` 由小写英文字母、数字和方括号 `'[]'` 组成
- `s` 保证是一个 **有效** 的输入。
- `s` 中所有整数的取值范围为 `[1, 300]` 

**题解**

```go
func decodeString(s string) string {
	// 栈，存储数字、左括号、字母
	stack := make([]string, 0)
	// 遍历字符串
	for i := 0; i < len(s); {
		cur := s[i]
		if cur >= '0' && cur <= '9' { // 数字
			// 获取连续数字（可能是2位数或者3位数）
			digits := getDigits(s, &i)
			// 将数字入栈
			stack = append(stack, digits)
		} else if (cur >= 'a' && cur <= 'z' || cur >= 'A' && cur <= 'Z') || cur == '[' { // 字母或者左括号
			// 入栈
			stack = append(stack, string(cur))
			i++
		} else { // 右括号
			sub := make([]string, 0)
			// 字母出栈，直到遇到左括号停止
			for stack[len(stack)-1] != "[" {
				sub = append(sub, stack[len(stack)-1])
				stack = stack[:len(stack)-1]
			}
			// 反转字母序列
			for j := 0; j < len(sub)/2; j++ {
				sub[j], sub[len(sub)-j-1] = sub[len(sub)-j-1], sub[j]
			}
			// 左括号出栈
			stack = stack[:len(stack)-1]
			// 取出栈顶元素（数字），并转化成 int 类型，这个就是前面取出字符串的倍数
			repeat, _ := strconv.Atoi(stack[len(stack)-1])
			// 数字出栈
			stack = stack[:len(stack)-1]
			// 将字符串数组转化成字符串，并生成重复的字符串
			t := strings.Repeat(strings.Join(sub, ""), repeat)
			// 字符串入栈
			stack = append(stack, t)
			i++
		}
	}
	return strings.Join(stack, "")
}

// 获取连续的数字
func getDigits(s string, i *int) string {
	ret := ""
	for ; s[*i] >= '0' && s[*i] <= '9'; *i++ {
		ret += string(s[*i])
	}
	return ret
}
```

### 哈希

#### [L1-简单] 两数之和

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

输入：nums = [3,2,4], target = 6
输出：[1,2]

输入：nums = [3,3], target = 6
输出：[0,1]
```

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

**题解**

哈希法:

```go
func twoSum(nums []int, target int) []int {
    hash := map[int]int{}
    for i, v := range nums {
        if p, ok := hash[target-v]; ok {
            return []int{p, i}
        }
        hash[v] = i
    }
    return nil
}
```

数组查找

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[]
     */
    function twoSum($nums, $target) {
        $count = count($nums);
        for ($i=0; $i<$count-1; $i++) {
            $tmp = $target - $nums[$i];
            $newnums = $nums;
            unset($newnums[$i]);
            $res = array_search($tmp, $newnums);
            if ($res !== false) {
                return array($i, $res);
                break;
            }
        }
    }
}
```

暴力：

```go
func twoSum(nums []int, target int) []int {
    n := len(nums)
    for i := 0; i < n - 1; i++ {
        for j := i + 1; j < n; j++ {
            if nums[i] + nums[j] == target {
                return []int{i, j}
            }
        }
    }
    return []int{}
}
```

PHP：

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[]
     */
    function twoSum($nums, $target) {
        $count = count($nums);
        for ($i=0; $i<$count-1; $i++) {
            for ($j=$i+1; $j<$count; $j++) {
            if(($nums[$i] + $nums[$j]) == $target ){
                    return array($i, $j);
                    break;
                }
            }
        }
    }
}
```

#### [L49-中等] 字母异位词分组

给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的所有字母得到的一个新单词。

**示例**

```
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]

输入: strs = [""]
输出: [[""]]

输入: strs = ["a"]
输出: [["a"]]
```

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` 仅包含小写字母

**题解**

```go
func groupAnagrams(strs []string) [][]string {
	hash := map[string][]string{}
	for _, str := range strs {
		// 将字符串转化成数组
		s := []byte(str)
		// 对字符串数组进行排序
		sort.Slice(s, func(i, j int) bool { return s[i] < s[j] })
		// 将字符串数组中的元素类型进行转化
		sortedStr := string(s)
		// 以字符串为key构建map
		hash[sortedStr] = append(hash[sortedStr], str)
	}
	// 重新组装成数组
	ans := make([][]string, 0, len(hash))
	for _, v := range hash {
		ans = append(ans, v)
	}
	return ans
}
```

#### [L128-中等] 最长连续序列

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。

输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

**题解**

```go
func longestConsecutive(nums []int) int {
	// 构建 hash 表
	hash := map[int]bool{}
	for _, v := range nums {
		hash[v] = true
	}
	max := 0
	// 遍历 hash 表
	for num := range hash {
		// 只取 num 为最小值的情况
		if !hash[num-1] {
			// 当前值
			current := num
			// 以当前值为最小值情况下的递增序列长度
			count := 1
			// 查找依次增加的序列
			for hash[current+1] {
				current++
				count++
			}
			// 更新最大序列长度值
			if count > max {
				max = count
			}
		}
	}
	return max
}
```

#### [L169-简单] 多数元素

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例**

```
输入：nums = [3,2,3]
输出：3

输入：nums = [2,2,1,1,1,2,2]
输出：2
```

- `n == nums.length`
- `1 <= n <= 5 * 104`
- `-109 <= nums[i] <= 109`

**进阶：**尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

**题解**

哈希表

```go
func majorityElement(nums []int) int {
    hash := map[int]int{}
    for _, num := range nums {
        if _, ok := hash[num]; ok {
            hash[num] = hash[num] + 1
        } else {
            hash[num] = 1
        }
    }
    maxKey := 0
    maxValue := 0
    for k, v := range hash {
        if v > maxValue {
            maxKey = k
            maxValue = v
        }
    }
    return maxKey
}
```

### 排序算法

#### [L56-中等] 合并区间

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

**示例**

```go
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

**题解**

1. 先根据数组左端大小进行排序
2. 遍历数组，依次比较每个数组，将前一个数组的右端 r 和后一个数组的左端 l 比较，若 `r < l`则说明不重叠

```go
func merge(intervals [][]int) [][]int {
    // 先安装数组左端元素从小到大进行排序
	n := len(intervals)
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			if intervals[i][0] > intervals[j][0] {
				intervals[i], intervals[j] = intervals[j], intervals[i]
			}
		}
	}
	res := make([][]int, 0)
	tmp := intervals[0]
	for i := 1; i < n; i++ {
		if tmp[1] < intervals[i][0] { // 没有重合
			res = append(res, tmp)
			tmp = intervals[i]
		} else { // 重合
			if tmp[1] < intervals[i][1] {
				tmp[1] = intervals[i][1]
			}
		}
	}
	res = append(res, tmp)
	return res
}
```

### 二分查找

#### [L35-简单] 搜索插入位置

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

**示例**

```go
输入: nums = [1,3,5,6], target = 5
输出: 2

输入: nums = [1,3,5,6], target = 2
输出: 1

输入: nums = [1,3,5,6], target = 7
输出: 4
```

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 为 **无重复元素** 的 **升序** 排列数组
- `-104 <= target <= 104`

**题解**

```go
func searchInsert(nums []int, target int) int {
	n := len(nums)
	left, right := 0, n-1
	insert := n
	for left <= right {
		mid := (right + left) / 2
		if target == nums[mid] { // 找到
			return mid
		} else if target < nums[mid] { // 目标值小于中间值，且找不到
			insert = mid
			right = mid - 1
		} else { // 目标值大于中间值，且找不到
			left = mid + 1
		}
	}
	return insert
}
```

#### [L33-中等] 搜索旋转排序数组

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

**示例**

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4

输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

**题解**

> 二分查找，外加一些判断条件

GO:

```go
func searchOrderArray(nums []int, target int) int {
	left, right := 0, len(nums)-1
	for left <= right {
		mid := (left + right) / 2
		if target == nums[mid] {
			return mid
		}
        // 左侧序列递增
		if nums[mid] >= nums[left] {
			if nums[left] <= target && target <= nums[mid] {// 在左序列中
				right = mid - 1
			} else {// 不在左序列
				left = mid + 1
			}
		} else {// 左序列非递增，则右序列递增
			if nums[mid] <= target && target <= nums[right] {// 在右序列中
				left = mid + 1
			} else {// 不在右序列中
				right = mid - 1
			}
		}
	}
	return -1
}
```

PHP:

```Php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer
     */
    function search($nums, $target) {
        if (!$nums) return -1;
        $low = 0;
        $high = count($nums) - 1;
        while ($low <= $high) {
            $mid = floor(($high - $low) / 2) + $low;
            if ($target == $nums[$mid]) return $mid;
            if ($nums[$low] <= $nums[$mid]) {
                if ($nums[$low] <= $target && $target < $nums[$mid]) {
                    $high = $mid - 1;
                } else {
                    $low = $mid + 1;
                }
            } else {
                if ($nums[$mid] < $target && $target <= $nums[$high]) {
                    $low = $mid + 1;
                } else {
                    $high = $mid - 1;
                }
            }
        }
        return -1;
    }
}
```

#### [L34-中等] 在排序数组中查找元素的第一个和最后一个位置

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(log n) 级别。

如果数组中不存在目标值，返回 [-1, -1]。

**示例**

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]

输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```

**题解**

> 由题意中复杂度O(log n) ，可知应该用二分思想

二分法：

GO：

```go
func searchRange(nums []int, target int) []int {
    res := []int{-1, -1}
    left, right := 0, len(nums) - 1
    for left <= right {
        mid := (left + right) / 2
        if target == nums[mid] {
            tmp := mid
            for mid >= left && target == nums[mid] {
                mid--
            }
            res[0] = mid + 1
            mid = tmp
            for mid <= right && target == nums[mid] {
                mid++
            }
            res[1] = mid - 1
            break
        } else if target > nums[mid] {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return res
}
```

PHP：

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[]
     */
    function searchRange($nums, $target) {
        $left = 0;
        $right = count($nums) - 1;
        $result = [-1, -1];
        while ($left <= $right) {
            $mid = floor(($right + $left) / 2);
            if($nums[$mid] == $target) {
                while ($mid >= $left && $nums[$mid] == $target) {
                    $mid--;
                }
                $result[0] = $mid + 1;
                $mid = floor(($right + $left) / 2);
                while ($mid <= $right && $nums[$mid] == $target) {
                    $mid++;
                }
                $result[1] = $mid - 1;
                break;
            } elseif($nums[$mid] > $target) {
                $right = $mid - 1;
            } else {
                $left = $mid + 1;
            }
        }
        return $result;
    }
}
```

暴力：

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[]
     */
    function searchRange($nums, $target) {
        $a = -1;
        $b = -1;
        for ($i = 0; $i < count($nums); $i++) {
            if($nums[$i] == $target){
                $a = $i;
                break;
            }
        }
        for ($j = count($nums) - 1; $j >= $a; $j--){
            if($nums[$j] == $target){
                $b= $j;
                break;
            }
        }
        return [$a,$b];
    }
}
```

#### [L74-中等] 搜索二维矩阵

给你一个满足下述两条属性的 `m x n` 整数矩阵：

- 每行中的整数从左到右按非严格递增顺序排列。
- 每行的第一个整数大于前一行的最后一个整数。

给你一个整数 `target` ，如果 `target` 在矩阵中，返回 `true` ；否则，返回 `false` 。

**示例**

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true

输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
输出：false
```

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-104 <= matrix[i][j], target <= 104`

**题解**

先对第一列进行二分查找，找出最后一个小于目标值的元素 row

因为，矩阵从上到下递增，从左到右递增，所以目标值只可能存在于 row 行中

再在 row 行中进行二分查找即可

```go
func searchMatrix(matrix [][]int, target int) bool {
	m, n := len(matrix), len(matrix[0])
	// 对第一列进行二分查找，找出最后一个小于目标值的元素
	left, right := 0, m-1
	for left < right {
		mid := (left + right) / 2
		if matrix[mid][0] == target {
			return true
		} else if matrix[mid][0] > target {
			right = mid - 1
		} else {
			left = mid + 1
		}
	}
	row := left
	// 保障 row 不会大于 target
	if matrix[row][0] > target {
		row--
	}
	if row < 0 {
		return false
	}
	// 在对应行中进行二分查找
	l, r := 0, n-1
	for l <= r {
		mid := (l + r) / 2
		if matrix[row][mid] == target {
			return true
		} else if matrix[row][mid] < target {
			l = mid + 1
		} else {
			r = mid - 1
		}
	}
	return false
}
```

#### [L153-中等] 搜索旋转排序数组中的最小值

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

**示例**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。

输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 3 次得到输入数组。

输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```

- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- `nums` 中的所有整数 **互不相同**
- `nums` 原来是一个升序排序的数组，并进行了 `1` 至 `n` 次旋转

**题解**

```go
func findMin(nums []int) int {
    l, r := 0, len(nums)-1
    min := nums[0]
    for l <= r {
        mid := (l + r) / 2
        // 中间值小于等于最右侧值，这部分序列左侧mid即是这部分的最小值，记录下
        if nums[mid] <= nums[r] {
            if nums[mid] < min {
                min = nums[mid]
            }
            r = mid - 1
        } else {
            // 中间值大于最右侧值，说明这部分序列中存在翻转点，即最小值
            l = mid + 1
        }
    }
    return min
}
```

### 递归

#### [L509-简单] 斐波那契数

**斐波那契数** （通常用 `F(n)` 表示）形成的序列称为 **斐波那契数列** 。该数列由 `0` 和 `1` 开始，后面的每一项数字都是前面两项数字的和。也就是：

```
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
```

给定 `n` ，请计算 `F(n)` 。

**示例**

```
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1

输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2

输入：n = 4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
```

- `0 <= n <= 30`

**题解**

递归：

```go
func fib(n int) int {
    if n < 2 {
        return n
    }
    return fib(n - 1) + fib(n - 2)
}
```

### 回溯算法

#### [L17-中等] 电话号码的字母组合

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

<img src="../../Image/oldimg/17_telephone_keypad.png" alt="17_telephone_keypad" style="zoom:50%;" />

**示例**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明：尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

输入：digits = "2"
输出：["a","b","c"]

输入：digits = ""
输出：[]
```

**题解**

**回溯法：**

<img src="../../Image/algorithm/lt17-letterCombinations.png" alt="image-20240624213814028" style="zoom:50%;" />

Go：

```go
var phoneMap map[string]string = map[string]string{
	"2": "abc",
	"3": "def",
	"4": "ghi",
	"5": "jkl",
	"6": "mno",
	"7": "pqrs",
	"8": "tuv",
	"9": "wxyz",
}

func backtrackLetter(digits string, index int, combination string, res *[]string) {
	if index == len(digits) {
		*res = append(*res, combination)
		return
	}
	digit := string(digits[index])
	letters := phoneMap[digit]
	for i := 0; i < len(letters); i++ {
		backtrackLetter(digits, index+1, combination+string(letters[i]), res)
	}
}

func letterCombinations(digits string) []string {
	if len(digits) == 0 {
		return []string{}
	}
	res := make([]string, 0)
	backtrackLetter(digits, 0, "", &res)
	return res
}
```

PHP：

```php
class Solution {
    public $res = [];
    public $str = "";
    public $array = [
        '2' => ['a', 'b', 'c'],
        '3' => ['d', 'e', 'f'],
        '4' => ['g', 'h', 'i'],
        '5' => ['j', 'k', 'l'],
        '6' => ['m', 'n', 'o'],
        '7' => ['p', 'q', 'r', 's'],
        '8' => ['t', 'u', 'v'],
        '9' => ['w', 'x', 'y', 'z'],
    ];

    /**
     * @param String $digits
     * @return String[]
     */
    function letterCombinations($digits) {
        if (!$digits) return [];
        $this->_dfs($digits, 0);
        return $this->res;
    }

    private function _dfs($digits, $step) {
        if ($step == strlen($digits)) {
            $this->res[] = $this->str;
            return;
        }
        $key = substr($digits, $step, 1);
        $chars = $this->array[$key];
        foreach ($chars as $v) {
            $this->str .=$v;
            $this->_dfs($digits, $step + 1);
            $this->str = substr($this->str, 0, strlen($this->str) - 1);
        }
    }
}
```

#### [L22-中等] 括号生成

给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。

**示例**

例如，给出 n = 3，生成结果为：

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

**题解**

回溯法

Go:

```go
func backtrackGenPas(left, right, n int, state string, res *[]string) {
	if left == n && right == n {
		*res = append(*res, state)
		return
	}
	if left < n {
		backtrackGenPas(left+1, right, n, state+"(", res)
	}
	if right < n && left > right {
		backtrackGenPas(left, right+1, n, state+")", res)
	}
}

func generateParenthesis(n int) []string {
	res := make([]string, 0)
	backtrackGenPas(0, 0, n, "", &res)
	return res
}
```

PHP：

```php
class Solution {
    public $list = [];
    /**
     * @param Integer $n
     * @return String[]
     */
    function generateParenthesis($n) {
        $this->_gen(0, 0, $n, '');
        return $this->list;
    }
    private function _gen($left, $right, $num, $result) {
        if ($left == $num && $right == $num) {
            array_push($this->list, $result);
            return;
        }
        if ($left < $num) {
            $this->_gen($left + 1, $right, $num, $result.'(');
        }
        if ($right < $num && $left > $right) {
            $this->_gen($left, $right + 1, $num, $result.')');
        }
    }
}
```

#### [L39-中等] 组合总和

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

**示例**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。

输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]

输入: candidates = [2], target = 1
输出: []
```

**提示：**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- `candidates` 的所有元素 **互不相同**
- `1 <= target <= 40`

**题解**

> 回溯算法
>

<img src="../../Image/algorithm/image-202403202038.png" style="zoom:75%;" />

代码实现：

```go
// 回溯
func backtraceCombinationSum(start, target int, state, choices []int, res *[][]int) {
	// 子集和等于 target 时，记录解
	if target == 0 {
		*res = append(*res, append([]int{}, state...))
		return
	}
	// 遍历所有选择
	// 剪枝二：从 start 开始遍历，避免生成重复子集
	for i := start; i < len(choices); i++ {
		// 剪枝一：若子集和超过 target ，则直接结束循环
		// 这是因为数组已排序，后边元素更大，子集和一定超过 target
		if target-choices[i] < 0 {
			break
		}
		// 更新状态
		state = append(state, choices[i])
		// // 进行下一轮选择
		backtraceCombinationSum(i, target-choices[i], state, choices, res)
		// 回退
		state = state[:len(state)-1]
	}
}

func combinationSum(candidates []int, target int) [][]int {
	// 先进行排序，为去重
	sort.Ints(candidates)
	res := make([][]int, 0)
	state := make([]int, 0)
	backtraceCombinationSum(0, target, state, candidates, &res)
	return res
}
```

#### [L46-简单] 全排列

给定一个 **没有重复 ** 数字的序列，返回其所有可能的全排列。

**示例**

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

输入：nums = [0,1]
输出：[[0,1],[1,0]]

输入：nums = [1]
输出：[[1]]
```

**题解**

回溯算法

![lt46-permute](../../Image/algorithm/lt46-permute.png)

Go：

```go
// 回溯算法
func backtracePermute(nums []int, state []int, selected []bool, res [][]int) [][]int {
	// 当状态长度等于元素数量时，记录解
	if len(nums) == len(state) {
		res = append(res, append([]int{}, state...))
		return res
	}
	// 遍历所有选择
	for i := 0; i < len(nums); i++ {
		if selected[i] {
			continue
		}
		// 更新状态
		selected[i] = true
		state = append(state, nums[i])
		// 进行下一轮选择
		res = backtracePermute(nums, state, selected, res)
		// 回退：撤销选择，恢复到之前的状态
		selected[i] = false
		state = state[:len(state)-1]
	}
	return res
}

// [L46-简单] 全排列
func permute(nums []int) [][]int {
	res := make([][]int, 0)
	state := make([]int, 0)
	selected := make([]bool, len(nums))
	return backtracePermute(nums, state, selected, res)
}
```

PHP：

```php
class Solution {
    public $res = [];
    /**
     * @param Integer[] $nums
     * @return Integer[][]
     */
    function permute($nums) {
        $this->do([], $nums);
        return $this->res;
    }
    function do($arr, $nums) {
        if (count($arr) == count($nums)) {
            array_push($this->res, $arr);
            return;
        }
        for ($i = 0; $i < count($nums); $i++) {
            if (in_array($nums[$i], $arr)) continue;
            array_push($arr, $nums[$i]);
            $this->do($arr, $nums);
            array_pop($arr);
        }
    }
}
```

#### [L78-中等] 子集

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

输入：nums = [0]
输出：[[],[0]]
```

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

**题解**

```go
// 回溯
func backtrackSubsets(nums []int, path []int, start int, res *[][]int) {
	// 记录结果
	*res = append(*res, append([]int{}, path...))
	// 遍历所有选择
	for i := start; i < len(nums); i++ {
		// 更新状态
		path = append(path, nums[i])
		// 进行下一次选择
		backtrackSubsets(nums, path, i+1, res)
		// 回退
		path = path[:len(path)-1]
	}
}

func subsets(nums []int) [][]int {
	res := make([][]int, 0)
	path := make([]int, 0)
	backtrackSubsets(nums, path, 0, &res)
	return res
}
```

#### [L79-中等] 单词搜索

给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例**

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
输出：true

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
输出：false
```

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` 和 `word` 仅由大小写英文字母组成

**题解**

```go
func searchWord(board [][]byte, word string) bool {
	m, n := len(board), len(board[0])
	words := []byte(word)
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if backtrackSW(board, words, i, j, 0) {
				return true
			}
		}
	}
	return false
}

func backtrackSW(board [][]byte, words []byte, i, j, k int) bool {
	// 越界
	if i < 0 || i >= len(board) || j < 0 || j >= len(board[0]) {
		return false
	}
	// 已访问过
	if board[i][j] == '0' {
		return false
	}
	// 当前字符串不匹配
	if board[i][j] != words[k] {
		return false
	}
	// 全部匹配通过
	if k == len(words)-1 {
		return true
	}
	// 标记访问过的路径
	board[i][j] = '0'
	// DFS遍历所有路径
	res := backtrackSW(board, words, i-1, j, k+1) ||
		backtrackSW(board, words, i+1, j, k+1) ||
		backtrackSW(board, words, i, j-1, k+1) ||
		backtrackSW(board, words, i, j+1, k+1)
	// 回退
	board[i][j] = words[k]
	return res
}
```

#### [L131-中等] 分割回文串

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 回文串。返回 `s` 所有可能的分割方案。

**示例**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]

输入：s = "a"
输出：[["a"]]
```

- `1 <= s.length <= 16`
- `s` 仅由小写英文字母组成

**题解**

```
func partition(s string) [][]string {
    path := make([]string, 0)
    ans := make([][]string, 0)
    backtrackPa(s, path, 0, &ans)
    return ans
}

func backtrackPa(s string, path []string, start int, ans *[][]string) {
    // 所有元素都遍历完成，将path添加到结果中
    if start == len(s) {
        *ans = append(*ans, append([]string{}, path...))
        return
    }
    // 遍历所有选择
    for i := start; i < len(s); i++ {
        if isPalindrome(s, start, i) {
            path = append(path, s[start:i+1])
            backtrackPa(s, path, i+1, ans)
            path = path[:len(path)-1]
        }
    }
    return
}

// 判断是否是回文串
func isPalindrome(s string, left, right int) bool {
    for left < right {
        if s[left] != s[right] {
            return false
        }
        left++
        right--
    }
    return true
}
```

### 动态规划

#### [L5-中等] 最长回文子串

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成

**示例**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

输入: "cbbd"
输出: "bb"
```

**题解**

**暴力法**

> 时间复杂度-O(n^3)
>
> 空间复杂度-O(1)

GO：

```go
func longestPalindrome(s string) string {
    n := len(s)
    if (n < 2) {
        return s
    }

    max, index := 1, 0
    for i := 0; i < n - 1; i++ {
        for j := i + 1; j < n; j++ {
            if j - i + 1 > max && isPalindrome(s, i, j) {
                max = j - i + 1
                index = i
            }
        }
    }

    return s[index: index+max]
}

func isPalindrome(s string, left int, right int) bool {
    for left < right {
        if s[left] != s[right] {
            return false
        }
        left++
        right--
    }
    return true
}
```

JavaScript：

```go
var longestPalindrome = function(s) {
    let n = s.length;
    if (n < 2) {
        return s;
    }

    let maxLen = 1;
    let index = 0;
    for (let i = 0; i < n - 1; i++) {
        for (let j = i + 1; j < n; j++) {
            if (j - i + 1 > maxLen && isPalindrome(s, i ,j)) {
                maxLen = j - i + 1;
                index = i;
            }
        }
    }

    return s.slice(index, index + maxLen);
};

function isPalindrome(str, left, right) {
    while(left < right) {
        if (str[left] !== str[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

**中心扩散法**

> 时间复杂度-O(n^2)
>
> 空间复杂度-O(1)

GO：

```go
func longestPalindrome(s string) string {
    n := len(s)
    if (n < 2) {
        return s
    }

    left, right := 0, 0
    for i := 0; i < n - 1; i++ {
        left1, right1 := expandAroundCenter(s, i, i);
        left2, right2 := expandAroundCenter(s, i, i + 1);
        if right1 - left1 > right - left {
            left, right = left1, right1
        }
        if right2 - left2 > right - left {
            left, right = left2, right2
        }
    }

    return s[left: right+1]
}

func expandAroundCenter(s string, left, right int) (int, int) {
    for left >= 0 && right < len(s) {
        if s[left] == s[right] {
            left--
            right++
        } else {
            break
        }
    }
    return left + 1, right - 1
}
```

PHP：

```php
class Solution {
    //定义全局
    public $res = "";
    public $max = 0;
    //比较左右
    private function diff($s, $left, $right) {
        while ($left >=0 && $right < strlen($s) && $s[$left] == $s[$right]) {
            if ($right - $left + 1 > $this->max) {
                $this->max = $right - $left + 1;
                $this->res = substr($s, $left, $right-$left+1);
            }
            $left--;
            $right++;
        }
    }
    /**
     * @param String $s
     * @return String
     */
    function longestPalindrome($s) {
        if (strlen($s) <= 1) return $s;
        for ($i = 0; $i < strlen($s); $i++) {
            $this->diff($s, $i, $i);
            $this->diff($s, $i, $i + 1);
        }
        return $this->res;
    }
}
```

**动态规划**

> 时间复杂度-O(n^2)
>
> 空间复杂度-O(n^2)

GO：

```go
func longestPalindrome(s string) string {
    n := len(s)
    if (n < 2) {
        return s
    }
    // 构建dp表：dp[i][j] 表示 s[i..j] 是否是回文串
    dp := make([][]bool, n)
    for i := range dp {
        dp[i] = make([]bool, n)
    }
    // 最大长度和开始下标
    max, index := 1, 0
    for j := 1; j < n; j++ {
        for i := 0; i < j; i++ {
            if s[i] != s[j] {
                dp[i][j] = false
            } else {
                if j - i < 3 {// 长度小于2，1个字母为回文串，2个字母相等也是回文串
                    dp[i][j] = true
                } else {
                    dp[i][j] = dp[i + 1][j - 1]
                }
            }
            // 更新回文长度和起始位置
            if dp[i][j] == true && j - i + 1 > max {
                max = j - i + 1
                index = i
            }
        }
    }
    return s[index: index+max]
}
```

PHP：

```php
class Solution {
    /**
     * @param String $s
     * @return String
     */
    function longestPalindrome($s) {
        if (strlen($s) <= 1) return $s;
        $res = $s[0];
        $max = 0;
        if ($s[0] == $s[1]) {
            $res = substr($s, 0, 2);
        }
        for ($j = 2; $j < strlen($s); $j++) {
            $dp[$j][$j] = true;
            for ($i = 0; $i < $j; $i++) {
                $dp[$i][$j] = $s[$i] == $s[$j] && ($j - $i <= 2 || $dp[$i + 1][$j - 1]);
                if ($dp[$i][$j] && $max < $j - $i + 1) {
                    $max = $j - $i + 1;
                    $res = substr($s, $i, $j - $i + 1);
                }
            }
        }
        return $res;
    }
}
```

#### [L70-简单] 爬楼梯

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶

输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

- `1 <= n <= 45`

**题解**

> 本题推导出公式为：*f*(*x*)=*f*(*x*−1)+*f*(*x*−2)，是斐波那契数，因此可以使用递归和动态规划求解，但是递归在本题中时间复杂度较高

动态规划：

```go
func climbStairs(n int) int {
	if n <= 2 {
		return n
	}
    dp := make([]int, n)
	dp[0] = 1
	dp[1] = 2
	for i := 2; i < n; i++ {
		dp[i] = dp[i-1] + dp[i-2]
	}
	return dp[n-1]
}

// 优化空间
func climbStairs2(n int) int {
    if n == 1 || n == 2 {
        return n
    }
    a, b := 1, 2
    // 状态转移：从较小子问题逐步求解较大子问题
    for i := 3; i <= n; i++ {
        a, b = b, a+b
    }
    return b
}
```

#### [L53-中等] 最大子数组和

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组**是数组中的一个连续部分

**示例**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。

输入：nums = [1]
输出：1

输入：nums = [5,4,-1,7,8]
输出：23
```

**题解**

动态规划

将问题分解成n个字问题：

- 子问题 1：以 −2-2−2 结尾的连续子数组的最大和是多少；
    - 以 −2 **结尾的**连续子数组是 `[-2]`，因此最大和就是 −2。
- 子问题 2：以 111 结尾的连续子数组的最大和是多少；
    - 以 1 结尾的连续子数组有 [-2,1] 和 [1] ，其中 [-2,1] 就是在「子问题 1」的后面加上 1 得到。−2+1=−1<1，因此「子问题 2」 的答案是 1。

- 子问题 3：以 −3-3−3 结尾的连续子数组的最大和是多少；
- 子问题 4：以 444 结尾的连续子数组的最大和是多少；
- 子问题 5：以 −1-1−1 结尾的连续子数组的最大和是多少；
- 子问题 6：以 222 结尾的连续子数组的最大和是多少；
- 子问题 7：以 111 结尾的连续子数组的最大和是多少；
- 子问题 8：以 −5-5−5 结尾的连续子数组的最大和是多少；
- 子问题 9：以 444 结尾的连续子数组的最大和是多少。

假设`dp[i]`表示以 `nums[i]` **结尾** 的 **连续** 子数组的最大和，则可以得出：

- 当 `dp[i - 1] > 0` 时 `dp[i] = dp[i - 1] + nums[i]`

- 当 `dp[i - 1] <= 0` 时 `dp[i] = nums[i]`

```go
func maxSubArray(nums []int) int {
    n := len(nums)
    dp := make([]int, n)
    dp[0] = nums[0]
    max := dp[0]
    for i := 1; i < n; i++ {
        if dp[i - 1] > 0 {
            dp[i] = dp[i - 1] + nums[i]
        } else {
            dp[i] = nums[i]
        }
        if dp[i] > max {
            max = dp[i]
        }
    }
    return max
}
```

#### [L62-中等] 不同路径

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

**示例**

```
输入：m = 3, n = 7
输出：28

输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下

输入：m = 7, n = 3
输出：28

输入：m = 3, n = 3
输出：6
```

- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 109`

**题解**

动态规划

`dp[i][j]` 是到达 `i, j` 最多路径，表达式：`dp[i][j] = dp[i-1][j] + dp[i`][j-1]

```go
func uniquePaths(m, n int) int {
	dp := make([][]int, m)
	for i := range dp {
		dp[i] = make([]int, n)
		dp[i][0] = 1
	}
	for j := 0; j < n; j++ {
		dp[0][j] = 1
	}
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			dp[i][j] = dp[i-1][j] + dp[i][j-1]
		}
	}
	return dp[m-1][n-1]
}
```

#### [L64-中等] 最小路径和

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

**示例**

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。

输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 200`

**题解**

动态规划

设 `dp[i][j]` 是到达 `i, j` 最多长路径，则：

- 当`i=0,j=0`时：`dp[0][0] = grid[0`][0]
- 当`i>0,j=0`时：`dp[i][0] = dp[i-1][0] + grid[i][0]`
- 当`i=0,j>0`时：`dp[0][j] = dp[0][j-1] + grid[0][j]`
- 当`i>0,j>0`时： `dp[i][j]=min(dp[i-1][j], dp[i][j-1]) + grid[i][j]`

```go
func minPathSum(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	dp := make([][]int, m)
	for i := range dp {
		dp[i] = make([]int, n)
	}
	dp[0][0] = grid[0][0]
	for i := 1; i < m; i++ {
		dp[i][0] = dp[i-1][0] + grid[i][0]
	}
	for j := 1; j < n; j++ {
		dp[0][j] = dp[0][j-1] + grid[0][j]
	}
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if dp[i-1][j] > dp[i][j-1] {
				dp[i][j] = dp[i][j-1] + grid[i][j]
			} else {
				dp[i][j] = dp[i-1][j] + grid[i][j]
			}
		}
	}
	return dp[m-1][n-1]
}

// 优化
func minPathSum(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	// 直接以 grid 为 dp 表结构
	// 初始化 dp[i][0]
	for i := 1; i < m; i++ {
		grid[i][0] = grid[i-1][0] + grid[i][0]
	}
	// 初始化 dp[0][j]
	for j := 1; j < n; j++ {
		grid[0][j] = grid[0][j-1] + grid[0][j]
	}
	// 求解
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if grid[i-1][j] > grid[i][j-1] {
				grid[i][j] = grid[i][j-1] + grid[i][j]
			} else {
				grid[i][j] = grid[i-1][j] + grid[i][j]
			}
		}
	}
	return grid[m-1][n-1]
}
```

#### [L118-简单] 杨辉三角

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![img](../../Image/algorithm/lt118-generate.gif)

**示例**

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]

输入: numRows = 1
输出: [[1]]
```

- `1 <= numRows <= 30`

**题解**

```go
func generate(numRows int) [][]int {
    ans := make([][]int, numRows)
    for i := 0; i < numRows; i++ {
        ans[i] = make([]int, i+1)
        ans[i][0] = 1
        ans[i][i] = 1
        for j := 1; j < i; j++ {
            ans[i][j] = ans[i-1][j-1] + ans[i-1][j]
        }
    }
    return ans
}
```

### 贪心算法

#### [L55-中等] 跳跃游戏

给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

**示例**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。

输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

**题解**

> 贪心算法
>

由题意可知：

尽可能到达最远位置（贪心）。
如果能到达某个位置，那一定能到达它前面的所有位置。

设 k 为最远可到达的位置，如果能到达当前位置（i < k），则最远位置`k=i+nums[i]`

若最远位置k已经 >= 数组最大长度，则肯定可以达到，可直接返回 true

```go
func canJump(nums []int) bool {
    k, n := 0, len(nums)
    for i := 0; i < n; i++ {
        if i > k {
            return false
        }
        if i + nums[i] > k {
            k = i + nums[i]
        }
        if k >= n - 1 {
            return true
        }
    }
    return true
}
```

#### [L121-简单] 买卖股票的最佳时机

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
     
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

**题解**

```go
func maxProfit(prices []int) int {
    minPro := int(1e5)
    maxPro := 0
    for i := 0; i < len(prices); i++ {
        if (prices[i] < minPro) {
            minPro = prices[i]
        }
        if (prices[i] - minPro > maxPro) {
            maxPro = prices[i] - minPro
        }
    }
    return maxPro
}
```

### 分治算法



### 双指针

#### [L283-简单] 移动零

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

**示例**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]

输入: nums = [0]
输出: [0]
```

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

**题解**

```go
func moveZeroes(nums []int)  {
    l, r, n := 0, 0, len(nums)
    for r < n {
        if nums[r] != 0 {
            nums[l], nums[r] = nums[r], nums[l]
            l++
        }
        r++
    }
}
```

#### [L11-中等] 盛最多水的容器

给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 *n* 的值至少为 2。

![img](../../Image/oldimg/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

**示例**

```
输入: [1,8,6,2,5,4,8,3,7]
输出: 49

输入：height = [1,1]
输出：1
```

**题解**

Go：

```go
func maxArea(height []int) int {
    n := len(height)
    left, right, max, tmp := 0, n - 1, 0, 0
    for left < right {
        if height[left] < height[right] {
            tmp = height[left] * (right - left)
            left++
        } else {
            tmp = height[right] * (right - left)
            right--
        }
        if tmp > max {
            max = tmp
        }
    }
    return max
}
```

PHP：

```php
class Solution {

    /**
     * @param Integer[] $height
     * @return Integer
     */
    function maxArea($height) {
        $left = 0;
        $right = count($height) -1;
        $maxarea = 0;
        while ($left < $right) {
        	$maxarea = max($maxarea, min($height[$left], $height[$right]) * ($right - $left));
        	if ($height[$left] < $height[$right]) {
        		$left++;
        	} else {
        		$right--;
        	}
        }
        return $maxarea;
    }
}
```

#### [L15-中等] 三数之和

给定一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例**

```
例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

**题解**

**排序+双指针**

Go：

```go
func threeSum(nums []int) [][]int {
    n := len(nums)
    if n < 3 {
        return [][]int{}
    }

    // 不能重复，所以先进行排序
    sort.Ints(nums)

    ret := make([][]int, 0)
    for i := 0; i < n - 2; i++ {
        if i > 0 && nums[i] == nums[i - 1] {
            continue
        }
        left, right := i + 1, n - 1
        need := 0 - nums[i]
        for left < right {
            if nums[left] + nums[right] == need {
                ret = append(ret, []int{nums[i], nums[left], nums[right]})
                for left < right && nums[left] == nums[left + 1] {
                    left++
                }
                for left < right && nums[right] == nums[right - 1] {
                    right--
                }
                left++
                right--
            } else if nums[left] + nums[right] < need {
                left++
            } else {
                right--
            }
        }
    }
    return ret
}
```

PHP：

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return Integer[][]
     */
    function threeSum($nums) {
        if (!$nums) return [];
        sort($nums);
        $ret = [];
        for ($i = 0; $i < count($nums) - 2; $i++) {
            if ($i > 0 && $nums[$i] == $nums[$i - 1]) continue;
            $left = $i + 1;
            $right = count($nums) - 1;
            
            $need = 0 - $nums[$i];
            
            while ($left < $right) {
                if ($nums[$left] + $nums[$right] == $need) {
                    array_push($ret, [$nums[$i], $nums[$left], $nums[$right]]);
                    while ($left < $right && $nums[$left] == $nums[$left + 1]) $left++;
                    while ($left < $right && $nums[$right] == $nums[$right - 1]) $right--;
                    $left++;
                    $right--;
                } else if ($nums[$left] + $nums[$right] > $need) {
                    $right--;
                } else {
                    $left++;
                }
            }
        }
        return $ret;
    }
}
```

#### [L75-中等] 颜色分类

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。

必须在不使用库内置的 sort 函数的情况下解决这个问题。

**示例**

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]

输入：nums = [2,0,1]
输出：[0,1,2]
```

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` 为 `0`、`1` 或 `2`

**题解**

双指针

设 p0、p2 分别为0元素末尾位置、2元素开始位置，在遍历过程中：

- 当 `nums[i] == 0`：则与交换`num[p0]`交换位置，并向右移动 p0 指针，继续遍历
- 当 `nums[i] == 2`：则与交换`num[p2]`交换位置，并向左移动 p2 指针
- 当 `nums[i] == 1`：不做交换，继续遍历

```go
func sortColors(nums []int) {
    // p0 为0元素的末尾，p2 为2元素的开始
    p0, p2 := 0, len(nums)-1
    // 当前位置
    i := 0
    for i <= p2 {
        // 为0，则和p0交换位置
        if nums[i] == 0 {
            nums[i], nums[p0] = nums[p0], nums[i]
            p0++
            i++
        } else if nums[i] == 2 { // 为2，则和 p2 交换位置
            nums[i], nums[p2] = nums[p2], nums[i]
            p2--
        } else { // 为1，则不交换，继续移动
            i++
        }
    }
}
```

#### [L42-困难] 接雨水

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例**

![img](../../Image/algorithm/lt42-rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 

输入：height = [4,2,0,3,2,5]
输出：9
```

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`

**题解**

动态规划：

```go
func trap(height []int) int {
	n := len(height)
	if n == 0 {
		return -1
	}

	leftMax := make([]int, n)
	leftMax[0] = height[0]
	for i := 1; i < n; i++ {
		if leftMax[i-1] > height[i] {
			leftMax[i] = leftMax[i-1]
		} else {
			leftMax[i] = height[i]
		}
	}

	rightMax := make([]int, n)
	rightMax[n-1] = height[n-1]
	for i := n - 2; i >= 0; i-- {
		if rightMax[i+1] > height[i] {
			rightMax[i] = rightMax[i+1]
		} else {
			rightMax[i] = height[i]
		}
	}
	ans := 0
	for i, h := range height {
		if leftMax[i] < rightMax[i] {
			ans += leftMax[i] - h
		} else {
			ans += rightMax[i] - h
		}
	}
	return ans
}
```

- 时间复杂度：O(n)
- 空间复杂度：O(n)

滑动窗口

> 在动态规划解法基础上进行优化

```go
func trap2(height []int) int {
	n := len(height)
	if n == 0 {
		return -1
	}
	ans := 0
	left, right, leftMax, rightMax := 0, n-1, 0, 0
	for left < right {
		if height[left] > leftMax {
			leftMax = height[left]
		}
		if height[right] > rightMax {
			rightMax = height[right]
		}
		if leftMax < rightMax {
			ans += leftMax - height[left]
			left++
		} else {
			ans += rightMax - height[right]
			right--
		}
	}
	return ans
}
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)

### 滑动窗口

#### [L3-中等] 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**题解**

滑动窗口

```go
func lengthOfLongestSubstring(s string) int {
    n := len(s)
    right, max := 0, 0
    // hash map，存储不重复的字符串，用于判断字符串是否出现过
    hash := make(map[byte]int)
    for i := 0; i < n; i++ {
        // 除了 i = 0 第一次，每一次移动左指针的时候都需要删除第一个hash元素
        if i != 0 {
            delete(hash, s[i-1])
        }
        // 不断移动右指针，直到出现重复字符退出
        for right < n && hash[s[right]] == 0 {
            hash[s[right]]++
            right++
        }
        // 更新最大值
        if right - i > max {
            max = right - i
        }
    }
    return max
}
```

PHP：

```php
class Solution {

    /**
     * @param String $s
     * @return Integer
     */
    function lengthOfLongestSubstring($s) {
        //边界
        if (!$s || strlen($s) == 0) return 0;
        //初始化
        $array= [];
        $ret = 0;
        $start = 0;
        //遍历
        for ($i = 0; $i < strlen($s); $i++) {
           if (isset($array[$s[$i]]) && $start <= $array[$s[$i]]) {
               $start = $array[$s[$i]] + 1;
           } else {
               $ret = max($ret, $i - $start + 1);
           }
            $array[$s[$i]] = $i;
        }
        return $ret;
    }
}
```

#### [L438-中等] 找到字符串中所有字母异位词

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**异位词** 指由相同字母重排列形成的字符串（包括相同的字符串）。

**示例**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。

输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

- `1 <= s.length, p.length <= 3 * 104`
- `s` 和 `p` 仅包含小写字母

**题解**

```go
func findAnagrams(s string, p string) []int {
	n, m := len(s), len(p)
	if n < m {
		return nil
	}

	// 维护2个m个元素的数组，统计每个字符出现的次数
	// 利用golang数组可以使用==比较的特性，判断2个窗口数组是否相等
	var sCount, pCount [123]int
	for i, ch := range p {
		sCount[s[i]]++
		pCount[ch]++
	}

	var ans []int
	if sCount == pCount {
		ans = append(ans, 0)
	}
	for i, ch := range s[:n-m] {
		sCount[ch]--
		sCount[s[i+m]]++
		if sCount == pCount {
			ans = append(ans, i+1)
		}
	}
	return ans
}
```

### 位运算

#### [L136-简单] 只出现一次的数字

你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

**示例**

```
输入：nums = [2,2,1]
输出：1

输入：nums = [4,1,2,1,2]
输出：4

输入：nums = [1]
输出：1
```

- `1 <= nums.length <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`
- 除了某个元素只出现一次以外，其余每个元素均出现两次。

**题解**

本题的注意点是要做到线性时间复杂度和常数空间复杂度

这里可以使用位运算中的异或运算（相同数异或运算为0，0和任何数异或运算为该数）

1. 任何数和 0 做异或运算，结果仍然是原来的数，即 a⊕0=a。
2. 任何数和其自身做异或运算，结果是 0，即 a⊕a=0。
3. 异或运算满足交换律和结合律，即 a⊕b⊕a=b⊕a⊕a=b⊕(a⊕a)=b⊕0=b。

```go
func singleNumber(nums []int) int {
    single := 0
    for _, num := range nums {
        single ^= num
    }
    return single
}
```

#### [L461-简单] 汉明距离

两个整数之间的 汉明距离 指的是这两个数字对应二进制位不同的位置的数目。

给你两个整数 `x` 和 `y`，计算并返回它们之间的汉明距离。

**示例**

```
输入：x = 1, y = 4
输出：2
解释：
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
上面的箭头指出了对应二进制位不同的位置。


输入：x = 3, y = 1
输出：1
```

- `0 <= x, y <= 231 - 1`

**题解**

```go
func hammingDistance(x int, y int) int {
    return bits.OnesCount(uint(x ^ y))
}
```

### 矩阵

#### [L48-中等] 旋转图像

给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

**示例**

```

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]

输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

**题解**

由题意可以得到规律：

对于矩阵中第 *i* 行的第 *j* 个元素，在旋转后，它出现在倒数第 *i* 列的第 *j* 个位置。

即：对于`matrix[row][col]`，在旋转后，它的新位置为 `matrix[col][n−row−1]`

```go
func rotate(matrix [][]int)  {
    n := len(matrix)
    newMatrix := make([][]int, n)
    for i := range newMatrix {
        newMatrix[i] = make([]int, n)
    }
    for i, row := range matrix {
        for j, col := range row {
            newMatrix[j][n-1-i] = col
        }
    }
    copy(matrix, newMatrix)
}
```

#### [L54-中等] 螺旋矩阵

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]

输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

**题解**

```go
func spiralOrder(matrix [][]int) []int {
    // 定义4个边界
    upper, down, left, right := 0, len(matrix)-1, 0, len(matrix[0]) - 1
    ans := make([]int, 0)
    for {
        // 向右移动直到最右
        for i := left; i <= right; i++ {
            ans = append(ans, matrix[upper][i])
        }
        // 重新设定上边界
        upper++
        if upper > down {
            break
        }
        // 向下
        for i := upper; i <= down; i++ {
            ans = append(ans, matrix[i][right])
        }
        // 重新设定右边界
        right--
        if right < left {
            break
        }
        // 向左
        for i := right; i >= left; i-- {
            ans = append(ans, matrix[down][i])
        }
        // 重新设定下边界
        down--
        if down < upper {
            break
        }
        // 向上
        for i := down; i >= upper; i-- {
            ans = append(ans, matrix[i][left])
        }
        // 重新设定左边界
        left++
        if left > right {
            break
        }
    }
    return ans
}
```

#### [L73-中等] 矩阵置零

给定一个 `m x n` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/原地算法)** 算法**。**

**示例**

```
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]

输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

**题解**

```go
func setZeroes(matrix [][]int)  {
    // 初始化两个数组，存放横纵每个位置是否为0
    row, col := make([]bool, len(matrix)), make([]bool, len(matrix[0]))
    // 遍历矩阵，进行标记
    for i := 0; i < len(matrix); i++ {
        for j := 0; j < len(matrix[i]); j++ {
            if matrix[i][j] == 0 {
                row[i] = true
                col[j] = true
            }
        }
    }
    // 遍历矩阵，对标记位置进行更新
    for i, v := range matrix {
        for j := range v {
            if row[i] || col[j] {
                v[j] = 0
            }
        }
    }
}
```

#### [L240-中等] 搜索二维矩阵II

编写一个高效的算法来搜索 `m x n` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

**示例**

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true

输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false
```

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-109 <= matrix[i][j] <= 109`
- 每行的所有元素从左到右升序排列
- 每列的所有元素从上到下升序排列
- `-109 <= target <= 109`

**题解**

```go
func searchMatrix(matrix [][]int, target int) bool {
    m, n := len(matrix), len(matrix[0])
    x, y := 0, n - 1
    for x < m && y >= 0 {
        if matrix[x][y] == target {
            return true
        } else if matrix[x][y] > target {
            y--
        } else {
            x++
        }
    }
    return false
}
```

### 其他

#### [L6-中等] Z 字变换

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"LEETCODEISHIRING"` 行数为 3 时，排列如下：

```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"LCIRETOESIIGEDHN"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

- `1 <= s.length <= 1000`
- `s` 由英文字母（小写和大写）、`','` 和 `'.'` 组成
- `1 <= numRows <= 1000`

**示例**

```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"

输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
L     D     R
E   O E   I I
E C   I H   N
T     S     G

输入：s = "A", numRows = 1
输出："A"
```

**题解**

方法一：

```php
class Solution {

    /**
     * @param String $s
     * @param Integer $numRows
     * @return String
     */
    function convert($s, $numRows) {
         if (strlen($s) == 1 || $numRows == 1) return $s;
        
        $ret = [];
        $pre = -1;
        $sign = 'down';
        for ($i = 0; $i < strlen($s); $i++) {
            if ($sign == 'up') {
                $ret[$pre - 1][] = $s[$i];
                $pre = $pre - 1;
                if ($pre == 0) {
                    $sign = 'down';
                }
            } else {
                $ret[$pre + 1][] = $s[$i];
                $pre = $pre + 1;
                if ($pre == $numRows - 1) {
                    $sign = 'up';
                }
            }
        }
        return implode('', array_map(function($a){return implode('', $a);}, $ret));
    }
}
```

方法二：

```php
class Solution {

    /**
     * @param String $s
     * @param Integer $numRows
     * @return String
     */
    function convert($s, $numRows) {
         if (strlen($s) == 1 || $numRows == 1) return $s;
        
        $ret = [];
        for ($i = 0; $i < strlen($s); $i++) {
            $index = $i % (2 * $numRows -2);
            $index = $index < $numRows ? $index : 2 * $numRows -2 - $index;
            $ret[$index][] = $s[$i];
        }
        return implode('', array_map(function($a){return implode('', $a);}, $ret));
    }
}
```

#### [L7-中等] 整数反转

给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−231,  231 − 1] ，就返回 0。

假设环境不允许存储 64 位整数（有符号或无符号）。

**示例**

```
输入: 123
输出: 321

输入: -123
输出: -321

输入: 120
输出: 21
```

**题解**

转成字符串处理（PHP）

```php
class Solution {

    /**
     * @param Integer $x
     * @return Integer
     */
    function reverse($x) {
        $l = strlen($x);
        $x = (string)$x;
        if ($x == 0) {
            return 0;
        }
        
        $flag = 1;
        if ($x[0] == '-') {
            $flag = -1;
        }

        $y = '';
        for($i = $l - 1; $i >= 0; $i--) {
            $y .= $x[$i];
        }
        $y = $flag * $y;

        if ($y > (pow(2,31)-1) || $y < pow(-2,31)) {
            return 0;
        }
        
        return $y;
    }
}
```

#### [L8-中等] 字符串转换整数

请你来实现一个 `atoi` 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

**说明：**

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，qing返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

**示例**

```
输入: "42"
输出: 42

输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
     
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
     
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

**题解**

方法一：

> 全部转化成字符串来解决

```php
class Solution {

    /**
     * @param String $str
     * @return Integer
     */
    function myAtoi($str) {
        $str = trim($str);
        if (!is_numeric($str[0]) && $str[0] != '-' && $str[0] != '+') return 0;
        $res = $str[0];
        
        for ($i = 1; $i < strlen($str); $i++) {
            if (is_numeric($str[$i])) {
                $res = $res.$str[$i];
            } else {
                break;
            }
        }
        $res = intval($res);
        if ($res >= pow(2, 31) - 1) {
            return pow(2, 31) - 1;
        } else if ($res <= pow(2, 31) * (-1)) {
            return pow(2, 31) * (-1);
        }
        return $res;
    }
}
```

方法二：

> 直接以整数的形势解题

```php
class Solution {

    /**
     * @param String $str
     * @return Integer
     */
    function myAtoi($str) {
        if (!$str) return 0;
        
        $str = trim($str);
        $first = $str[0];
        $sign = 1;
        $res = 0;
        if ($first == '+') {
            $start = 1;
        } else if ($first == '-') {
            $start = 1;
            $sign = -1;
        } else {
            $start = 0;
        }
        for ($i = $start; $i < strlen($str); $i++) {
            if (!is_numeric($str[$i])) {
                return $res * $sign;
            }
            $res = $res * 10 + $str[$i];
            if ($res >= pow(2, 31)) {
                if ($sign > 0) {
                    return pow(2, 31) - 1 * $sign;
                }
                return pow(2, 31) * $sign;
            }
        }
        return $res * $sign;
    }
}
```

#### [L9-简单] 回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

进阶：你能不将整数转为字符串来解决这个问题吗？

**示例**

```
输入: 121
输出: true

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

**题解**

**方法一：转化成字符串**

```php
class Solution {

    /**
     * @param Integer $x
     * @return Boolean
     */
    function isPalindrome($x) {
        $len = strlen($x);
        $x = (string)$x;
        for ($i=$len-1; $i>=0; $i--) {
            $y.=$x[$i];
        }
        if ($x == $y) {
            return true;
        } else {
            return false;
        }
    }
}
```

**方法二：**

```php
class Solution {

    /**
     * @param Integer $x
     * @return Boolean
     */
    function isPalindrome($x) {
        // 所有负数返回 false
        // 10的倍数返回false
        if ($x < 0 || ($x % 10 == 0 && $x != 0)) {
            return false;
        }
        // %10得到最后一位数字
        // 先/10再%10得到倒数第二位数字，以此类推
        $y = 0;
        while ($x > $y) {
            $y = $y * 10 + $x % 10;
            $x /= 10;
            $x = (int)$x;
        }
        $new = $y/10;
        $new = (int)$new;
        return $x == $y || $x == $new;
    }
}
```

#### [L12-中等] 整数转罗马数字

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

**示例**

```
输入: 3
输出: "III"

输入: 4
输出: "IV"

输入: 9
输出: "IX"

输入: 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.

输入: 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

**题解**

```php
class Solution {

    /**
     * @param Integer $num
     * @return String
     */
    function intToRoman($num) {
        $arr1 = [1000,900,500,400,100,90,50,40,10,9,5,4,1];
        $arr2 = ["M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"];
        $res = '';
        for($i = 0; $i < 13; $i++){
            while($num >= $arr1[$i]){
                $num -= $arr1[$i];
                $res = $res.$arr2[$i];
            }
        }
        return $res;
    }
}
```

#### [L13-简单] 罗马数字转整数

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

**示例**

```
输入: "III"
输出: 3

输入: "IV"
输出: 4

输入: "IX"
输出: 9

输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.

输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4
```

**题解**

如果当前字符代表的值不小于其右边，就加上该值；否则就减去该值。以此类推到最左边的数，最终得到的结果即是答案

```php
class Solution {

    /**
     * @param String $s
     * @return Integer
     */
    function romanToInt($s) {
        $tmp = [
            'I' => 1,
            'V' => 5,
            'X' => 10,
            'L' => 50,
            'C' => 100,
            'D' => 500,
            'M' => 1000,
        ];
        $s = (string)$s;
        $len = strlen($s);
        for ($i=0; $i<$len; $i++) {
            if ($tmp[$s[$i]] >= $tmp[$s[$i+1]]) {
                $ans+=$tmp[$s[$i]];
            } else {
                $ans-=$tmp[$s[$i]];
            }
        }
        return $ans;
    }
}
```

#### [L14-简单] 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

说明：所有输入只包含小写字母 `a-z` 。

**示例**

```
输入: ["flower","flow","flight"]
输出: "fl"

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**题解**

获取第一个元素，以第一个元素为基准点，遍历数组，依次和第一个元素对比，获取相同元素个数

Golang：

```go
func longestCommonPrefix(strs []string) string {
    if len(strs) == 0 {
        return ""
    }
    for i := 0; i < len(strs[0]); i++ {
        for j := 1; j < len(strs); j++ {
            if i == len(strs[j]) || strs[j][i] != strs[0][i] {
                return strs[0][:i]
            }
        }
    }
    return strs[0]
}
```

PHP：

```php
class Solution {

    /**
     * @param String[] $strs
     * @return String
     */
    function longestCommonPrefix($strs) {
        $len = count($strs);
        $first = $strs[0];
        $firstLen = strlen($first);
        for ($i = 1;$i < $len; $i++) {
            $tempArr = str_split($strs[$i]);
            $min = min($firstLen, count($tempArr));
            $tempLen = 0;
            for ($j = 0; $j < $min; $j++) {
                if ($first[$j] == $tempArr[$j]) {
                    $tempLen++;
                } else {
                    break;
                }
            }
            $firstLen > $tempLen && $firstLen = $tempLen;
        }
        return substr($first, 0, $firstLen);
    }
}
```

#### [L16-中等] 最接近的三数之和

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

**示例**

例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).

**题解**

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer
     */
    function threeSumClosest($nums, $target) {
        if (!nums) return [];
        $length = count($nums);
        sort($nums);
        $min = $nums[0] + $nums[1] + $nums[2];
        for ($i = 0; $i < $length - 2; $i++) {
            $left = $i + 1;
            $right = $length - 1;
            while ($left < $right) {
                $sum = $nums[$left] + $nums[$right] + $nums[$i];
                if ($sum < $target) {
                    $left++;
                } else {
                    $right--;
                }
                if (abs($sum - $target) < abs($min - $target)) {
                    $min = $sum;
                }
            }
        }
        return $min;
    }
}
```

#### [L18-中等] 四数之和

给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：答案中不可以包含重复的四元组。

**示例**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

**题解**

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[][]
     */
    function fourSum($nums, $target) {
        if (!$nums) return [];
        sort($nums);
        $ret = [];
        for ($i = 0; $i < count($nums) - 3; $i++) {
            if ($i > 0 && $nums[$i] == $nums[$i - 1]) continue;
            for ($j = $i + 1; $j < count($nums) - 2; $j++) {
                if ($j > ($i + 1) && $nums[$j] == $nums[$j - 1]) continue;
                $left = $j + 1;
                $right = count($nums) - 1;
                while ($left < $right) {
                    $sum = $nums[$i] + $nums[$j] + $nums[$left] + $nums[$right];
                    if ($sum == $target) {
                        array_push($ret, [$nums[$i], $nums[$j], $nums[$left], $nums[$right]]);
                        while ($left < $right && $nums[$left] == $nums[$left + 1]) $left++;
                        while ($left < $right && $nums[$right] == $nums[$right - 1]) $right--;
                        $left++;
                        $right--;
                    } else if ($sum > $target) {
                        $right--;
                    } else {
                        $left++;
                    }
                }
            }
        }
        return $ret;
    }
}
```

#### [L26-中等] 删除排序数组中的重复项

给定一个排序数组，你需要在**原地**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**原地修改输入数组**并在使用 O(1) 额外空间的条件下完成。

**示例**

```
给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
你不需要考虑数组中超出新长度后面的元素。

给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
你不需要考虑数组中超出新长度后面的元素。
```

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**“引用”**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**题解**

两两比较，如果不相等则插入nums中

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function removeDuplicates(&$nums) {
        if (!$nums) return 0;
        $i = 0;
        for ($j = 1; $j < count($nums); $j++) {
            if ($nums[$j] != $nums[$i]) {
                $i++;
                $nums[$i] = $nums[$j];
            }
        }
        return $i + 1;
    }
}
```

#### [L27-简单] 移除元素

给定一个数组 *nums* 和一个值 *val*，你需要**原地**移除所有数值等于 *val* 的元素，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**原地修改输入数组**并在使用 O(1) 额外空间的条件下完成。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**示例**

```
给定 nums = [3,2,2,3], val = 3,
函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
你不需要考虑数组中超出新长度后面的元素。

给定 nums = [0,1,2,2,3,0,4,2], val = 2,
函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
注意这五个元素可为任意顺序。
你不需要考虑数组中超出新长度后面的元素。
```

**题解**

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $val
     * @return Integer
     */
    function removeElement(&$nums, $val) {
        $n = 0;
        for ($i = 0; $i < count($nums); $i++) {
            if (!$nums) return 0;
            if ($nums[$i] != $val) {
                $nums[$n] = $nums[$i];
                $n++;
            }
        }
        return $n;
    }
}
```

#### [L28-简单] 实现strStr()

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  **-1**。

**示例**

```
输入: haystack = "hello", needle = "ll"
输出: 2

输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

**题解**

```php
class Solution {

    /**
     * @param String $haystack
     * @param String $needle
     * @return Integer
     */
    function strStr($haystack, $needle) {
       for ($i = 0; $i <= strlen($haystack) - strlen($needle); $i++) {
           $str = substr($haystack, $i, strlen($needle));
           if ($str == $needle) {
               return $i;
           }
       }
        return -1;
    }
}
```

#### [L29-中等] 两数相除

给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 dividend 除以除数 divisor 得到的商。

**示例**

```
输入: dividend = 10, divisor = 3
输出: 3

输入: dividend = 7, divisor = -3
输出: -2
```

被除数和除数均为 32 位有符号整数。
除数不为 0。
假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231,  231 − 1]。本题中，如果除法结果溢出，则返回 231 − 1。

**题解**

```php
class Solution {

    /**
     * @param Integer $dividend
     * @param Integer $divisor
     * @return Integer
     */
    function divide($dividend, $divisor) {
        $sign = 1;
        if (($dividend < 0) && ($divisor > 0) || ($dividend > 0) && ($divisor < 0)) {
            $sign = -1;
        }
        $dividend = abs($dividend);
        $divisor = abs($divisor);
        
        if ($dividend < $divisor) return 0;
        $sum  = $divisor;
        $multi = 1;
        while ($sum + $sum < $dividend) {
            $sum += $sum;
            $multi += $multi;
        }
        
        $data = $multi + $this->divide($dividend - $sum, $divisor);
        $num = $sign < 1 ? 0 - $data : $data;
        
        if ($num > pow(2, 31) - 1) return pow(2, 31) - 1;
        if ($num < pow(-2, 31)) return pow(-2, 31);
        
        return $num;
    }
}
```

#### [L31-中等] 下一个排列

整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**[ 原地 ](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

**示例**

```
输入：nums = [1,2,3]
输出：[1,3,2]

输入：nums = [3,2,1]
输出：[1,2,3]

输入：nums = [1,1,5]
输出：[1,5,1]
```

**题解**

解题思路：

1. 第一次从右侧开始遍历，找到第一个较小值 `nums[i]`
2. 第二次从右侧开始遍历，找出第一个大于较小值的数值 `nums[j]`
3. 交换 `nums[i]` 和  `nums[j]`
4. 翻转  `nums[i]` 之后的序列



![Next Permutation](../../Image/oldimg/1df4ae7eb275ba4ab944521f99c84d782d17df804d5c15e249881bafcf106173-file_1555696082944.gif)

GO：

```go
func nextPermutation(nums []int) {
	n := len(nums)
	i := n - 2
	// 第一次从右侧开始遍历，找出最靠近右侧的较小值
	for i >= 0 && nums[i] >= nums[i+1] {
		i--
	}
	if i >= 0 {
		j := n - 1
		// 第二次遍历，找出最靠右的大于较小值的第一个数
		for j >= 0 && nums[i] >= nums[j] {
			j--
		}
		// 交换两个数的位置
		nums[i], nums[j] = nums[j], nums[i]
	}
	// 将右侧翻转
	reverse(nums[i+1:])
	//fmt.Println(nums)
}

// 翻转数组
func reverse(a []int) {
	for i, n := 0, len(a); i < n/2; i++ {
		a[i], a[n-1-i] = a[n-1-i], a[i]
	}
}
```

PHP：

```php
class Solution {

    /**
     * @param Integer[] $nums
     * @return NULL
     */
    function nextPermutation(&$nums) {
        $n = count($nums);
        for ($i = $n - 2; $i >= 0; $i--) {
            if ($nums[$i + 1] > $nums[$i]) {
                for ($j = $n - 1; $j >= 0; $j--) {
                    if ($nums[$j] > $nums[$i]) {
                        break;
                    }
                }
                $temp = $nums[$i];
                $nums[$i] = $nums[$j];
                $nums[$j] = $temp;
                
                $left = $i + 1;
                $right = $n - 1;
                return $this->reverse($nums, $left, $right);
            }
        }
        return $this->reverse($nums, 0, $n - 1);
    }
    //反转函数
    function reverse(&$nums, $left, $right) {
        while ($left < $right) {
            $temp = $nums[$left];
            $nums[$left] = $nums[$right];
            $nums[$right] = $temp;
            
            $left++;
            $right--;
        }
    }
}
```

#### [L189-中等] 轮转数组

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

**示例：**

```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]

输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
```

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

**题解：**

根据k值切分两个数组，再合并数组，再将新数组copy给原数组

```go
func rotateArray(nums []int, k int) {
	n := len(nums)
	// 处理 k > n 的情况
	k = k % n
	arr1 := nums[:n-k]
	arr2 := nums[n-k:]
	ans := append(arr2, arr1...)
	copy(nums, ans)
}
```

#### [L238-中等] 除自身以外数组的乘积

给你一个整数数组 `nums`，返回 *数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积* 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(n)` 时间复杂度内完成此题。

**示例：**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]

输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内

**题解：**

`ans[i]`的乘机等于`ans[i-1]` * `ans[i+1]`，因此，需要计算两侧每个位置的乘机值

```go
func productExceptSelf(nums []int) []int {
	n := len(nums)
	// 初始化两个数组，分别存储 i 左右两侧的乘机
	lArr, rArr := make([]int, n), make([]int, n)
	lArr[0] = 1
	for i := 1; i < n; i++ {
		lArr[i] = lArr[i-1] * nums[i-1]
	}
	rArr[n-1] = 1
	for i := n - 2; i >= 0; i-- {
		rArr[i] = rArr[i+1] * nums[i+1]
	}
	// i 的值为左右两侧乘机
	ans := make([]int, n)
	for i := 0; i < n; i++ {
		ans[i] = lArr[i] * rArr[i]
	}
	return ans
}
```

#### [L338-简单] 比特位计数

给你一个整数 `n` ，对于 `0 <= i <= n` 中的每个 `i` ，计算其二进制表示中 **`1` 的个数** ，返回一个长度为 `n + 1` 的数组 `ans` 作为答案。

**示例**

```
输入：n = 2
输出：[0,1,1]
解释：
0 --> 0
1 --> 1
2 --> 10

输入：n = 5
输出：[0,1,1,2,1,2]
解释：
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```

- `0 <= n <= 105`

**题解**

```go
func onesCount(x int) (ones int) {
    for ; x > 0; x &= x - 1 {
        ones++
    }
    return
}

func countBits(n int) []int {
    bits := make([]int, n+1)
    for i := range bits {
        bits[i] = onesCount(i)
    }
    return bits
}
```

#### [L448-简单] 找到数组中所有消失的数字

给你一个含 `n` 个整数的数组 `nums` ，其中 `nums[i]` 在区间 `[1, n]` 内。请你找出所有在 `[1, n]` 范围内但没有出现在 `nums` 中的数字，并以数组的形式返回结果。

**示例**

```
输入：nums = [4,3,2,7,8,2,3,1]
输出：[5,6]

输入：nums = [1,1]
输出：[2]
```

- `n == nums.length`
- `1 <= n <= 105`
- `1 <= nums[i] <= n`

**题解**

```go
func findDisappearedNumbers(nums []int) []int {
    res := []int{}
    n := len(nums)
    for i := 1; i <= n; i++ {
        flag := false
        for j := 0; j < n; j++ {
            if i == nums[j] {
                flag = true
                break
            }
        }
        if flag == false {
            res = append(res, i)
        }
    }
    return res
}
```

#### [L560-中等] 和为 K 的子数组

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。

子数组是数组中元素的连续非空序列。

**示例**

```go
输入：nums = [1,1,1], k = 2
输出：2

输入：nums = [1,2,3], k = 3
输出：2
```

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`

**代码实现**

```go
func subarraySum(nums []int, k int) int {
    count, sum := 0, 0
	prefixSums := make(map[int]int)
	prefixSums[0] = 1

	for _, num := range nums {
		sum += num
		if val, ok := prefixSums[sum-k]; ok {
			count += val
		}
		prefixSums[sum]++
	}
	return count
}
```

