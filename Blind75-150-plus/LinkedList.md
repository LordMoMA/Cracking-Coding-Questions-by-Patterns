[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

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
        nextTemp := curr.Next
        curr.Next = prev
        prev = curr
        curr = nextTemp
    }
    return prev
}
```

[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

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
    if list1.Val < list2.Val {
        list1.Next = mergeTwoLists(list1.Next, list2)
        return list1
    } else {
        list2.Next = mergeTwoLists(list1, list2.Next)
        return list2
    }
}
```

[138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 *     Random *Node
 * }
 */

func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    m := make(map[*Node]*Node)
    curr := head
    for curr != nil {
        m[curr] = &Node{Val: curr.Val}
        curr = curr.Next
    }
    curr = head
    for curr != nil {
        m[curr].Next = m[curr.Next]
        m[curr].Random = m[curr.Random]
        curr = curr.Next
    }
    return m[head]
}
```
If there are some data in it, it would look like a mapping between original nodes and their corresponding copied nodes.

Let's say we have a linked list with three nodes:

Node1(Val: 1) -> Node2(Val: 2) -> Node3(Val: 3)

After running the first loop in the copyRandomList function, the map would look like this:

Node1 -> CopyNode1(Val: 1)
Node2 -> CopyNode2(Val: 2)
Node3 -> CopyNode3(Val: 3)

Another expression:

The structure of the for loop in Go is for initialization; condition; post { }.

initialization: This is executed before the first iteration. Here, node := head initializes the node variable to the head of the list.
condition: This is the test condition checked before every iteration. If this condition evaluates to true, the loop continues; otherwise, it stops. Here, node != nil checks if the node is not nil.
post: This is executed after every iteration. Here, node = node.Next moves the node to the next node in the list.

```go
type Node struct {
    Val    int
    Next   *Node
    Random *Node
}

func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }

    nodeMap := make(map[*Node]*Node)

    // First pass: make a copy of each node and store it in the map
    for node := head; node != nil; node = node.Next {
        nodeMap[node] = &Node{Val: node.Val}
    }

    // Second pass: set the next and random pointers for each copied node
    for node := head; node != nil; node = node.Next {
        if node.Next != nil {
            nodeMap[node].Next = nodeMap[node.Next]
        }
        if node.Random != nil {
            nodeMap[node].Random = nodeMap[node.Random]
        }
    }

    // Return the copied head node
    return nodeMap[head]
}
```

The below solution will cause:

input: [[7,null],[13,0],[11,4],[10,2],[1,0]]
output: [[7,null]]
expected: [[7,null],[13,0],[11,4],[10,2],[1,0]]


```go
func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    m := make(map[*Node]*Node)
    curr := head
    for curr != nil {
        m[curr] = &Node{Val: curr.Val}
        m[curr].Next = m[curr.Next]
        m[curr].Random = m[curr.Random]
        curr = curr.Next
    }

    return m[head]
}
```

The reason you can't combine the two loops into one in the copyRandomList function is because when you're creating the copy of each node in the first loop, you don't yet have the copies of the next and random nodes to assign to m[curr].Next and m[curr].Random.

In the first loop:

You're creating a new node for each node in the original list and storing it in the map m. At this point, you don't have the copies of the next and random nodes yet, so you can't assign them to m[curr].Next and m[curr].Random.

In the second loop:

You're setting the next and random pointers for each copied node. At this point, you already have the copies of the next and random nodes, so you can assign them to m[curr].Next and m[curr].Random.

If you try to combine the two loops into one, you'll end up with a null pointer exception when you try to access m[curr.Next] and m[curr.Random] in the first loop, because the copies of the next and random nodes don't exist yet.

[2. Add Two Numbers](http://leetcode.com/problems/add-two-numbers)

Initialize a variable carry to keep track of the carry from the addition. Set the dummy node to simplify the code. Move the curr pointer to the next node in each iteration. The carry is the sum divided by 10, and the new node is the sum modulo 10.

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{}
    curr := dummy
    carry := 0
    for l1 != nil || l2 != nil {
        sum := carry
        if l1 != nil {
            sum += l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            sum += l2.Val
            l2 = l2.Next
        }
        carry = sum / 10
        curr.Next = &ListNode{Val: sum % 10}
        curr = curr.Next
    }
    if carry > 0 {
        curr.Next = &ListNode{Val: carry}
    }
    return dummy.Next
}
```

Input: l1 = [2,4,3], l2 = [5,6,4]

The first digits are 2 and 5. The sum is 7. Since 7 is less than 10, there is no carry. The first digit of the result is 7.

The second digits are 4 and 6. The sum is 10. Since 10 is equal to 10, there is a carry of 1 to the next digit. The second digit of the result is 0 (10 % 10).

The third digits are 3 and 4. The sum is 7, plus the carry of 1 from the previous step, which makes it 8. Since 8 is less than 10, there is no carry. The third digit of the result is 8.

So, the result is [7,0,8], which represents the number 807.

The line if carry > 0 { curr.Next = &ListNode{Val: carry} } outside the loop is for the case where you have a carry left after you've processed all digits from both lists. This would happen, for example, if you were adding [9,9] and [2], which would give you [1,0,1]. The final 1 comes from the carry after adding the two 9's.

[146. LRU Cache](http://leetcode.com/problems/lru-cache)

[LRU Knowledge](https://books.halfrost.com/leetcode/ChapterThree/LRUCache/)

In the context of this LRU Cache implementation, the head and tail nodes are dummy nodes. They are placeholders that make the implementation easier by eliminating the need to check for null pointers.

So, in the list head <-> 1 <-> 2 <-> 3 <-> 4 <-> 5 <-> tail, head and tail are dummy nodes. The actual data is stored in nodes 1 through 5.

When we say "remove the tail", we're referring to removing the node before the tail dummy node, which is 5 in this case.

```go
type DLinkedNode struct {
    key, value int
    prev, next *DLinkedNode
}

func initDLinkedNode(key, value int) *DLinkedNode {
    return &DLinkedNode{
        key:   key,
        value: value,
    }
}

type LRUCache struct {
    size       int
    capacity   int
    cache      map[int]*DLinkedNode
    head, tail *DLinkedNode
}

func Constructor(capacity int) LRUCache {
    l := LRUCache{
        capacity: capacity,
        cache:    map[int]*DLinkedNode{},
        head:     initDLinkedNode(0, 0),
        tail:     initDLinkedNode(0, 0),
    }
    l.head.next = l.tail
    l.tail.prev = l.head
    return l
}

func (this *LRUCache) Get(key int) int {
    if _, ok := this.cache[key]; !ok {
        return -1
    }
    node := this.cache[key]
    this.moveToHead(node)
    return node.value
}

func (this *LRUCache) Put(key int, value int) {
    if _, ok := this.cache[key]; !ok {
        node := initDLinkedNode(key, value)
        this.cache[key] = node
        this.addToHead(node)  // attention, not moveToHead()!
        this.size++
        if this.size > this.capacity {
            removed := this.removeTail()
            delete(this.cache, removed.key) // attention: delete usage
            this.size--
        }
    } else {
        node := this.cache[key]
        node.value = value
        this.moveToHead(node)
    }
}

func (this *LRUCache) addToHead(node *DLinkedNode) {
    node.prev = this.head
    node.next = this.head.next
    this.head.next.prev = node
    this.head.next = node
}

func (this *LRUCache) removeNode(node *DLinkedNode) {
    node.prev.next = node.next
    node.next.prev = node.prev
}

func (this *LRUCache) moveToHead(node *DLinkedNode) {
    this.removeNode(node)
    this.addToHead(node)
}

func (this *LRUCache) removeTail() *DLinkedNode {
    node := this.tail.prev
    this.removeNode(node)
    return node
}
```

In Go, the delete built-in function is used to remove an entry from a map.

removeNode 3 from 1 <-> 2 <-> 3 <-> 4 <-> 5:

1 <-> 2 -> 4 <-> 5
       3 <->

1 <-> 2 <-> 4 <-> 5
       3

```go
type LRUCache struct {
    capacity int
    cache    map[int]*DListNode
    head     *DListNode // Points to the least recently used node
    tail     *DListNode // Points to the most recently used node
}

type DListNode struct {
    key, value int
    prev, next *DListNode
}

func Constructor(capacity int) LRUCache {
    return LRUCache{
        capacity: capacity,
        cache:    make(map[int]*DListNode),
        head:     &DListNode{},
        tail:     &DListNode{},
    }
}

func (this *LRUCache) Get(key int) int {
    if node, ok := this.cache[key]; ok {
        this.moveToFront(node)
        return node.value
    }
    return -1
}

func (this *LRUCache) Put(key int, value int) {
    if node, ok := this.cache[key]; ok {
        node.value = value
        this.moveToFront(node)
        return
    }

    newNode := &DListNode{key: key, value: value}
    this.cache[key] = newNode
    this.addToFront(newNode)

    if len(this.cache) > this.capacity {
        this.removeTail()
    }
}

func (this *LRUCache) addToFront(node *DListNode) {
    node.prev = this.head
    node.next = this.head.next
    this.head.next = node
    if node.next != nil {
        node.next.prev = node
    } else {
        this.tail = node
    }
}

func (this *LRUCache) removeTail() {
    delete(this.cache, this.tail.key)
    this.tail = this.tail.prev
    this.tail.next = nil
}

func (this *LRUCache) moveToFront(node *DListNode) {
    if node == this.head.next {
        return // Already at the front
    }
    // Remove node from its current position
    node.prev.next = node.next
    if node.next != nil {
        node.next.prev = node.prev
    } else {
        this.tail = node.prev
    }
    this.addToFront(node)
}

```

Solution 2 (consider rewrite container/list with your own method ):

```go
import "container/list"

type LRUCache struct {
    Cap int
    Keys map[int] *list.Element
    List *list.List
}

type pair struct {
    K, V int
}

func Constructor(capacity int) LRUCache {
    return LRUCache{
        Cap: capacity,
        Keys: make(map[int]*list.Element),
        List: list.New(),
    }
}


func (this *LRUCache) Get(key int) int {
    if el, ok := this.Keys[key]; ok {
        this.List.MoveToFront(el)
        return el.Value.(pair).V
    }
    return -1
}


func (this *LRUCache) Put(key int, value int)  {
    if el, ok := this.Keys[key]; ok {
        el.Value = pair{K: key, V: value}
        this.List.MoveToFront(el)
    } else {
        el := this.List.PushFront(pair{K: key, V: value})
        this.Keys[key] = el
    }
    if this.List.Len() > this.Cap {
        el := this.List.Back()
        this.List.Remove(el)
        delete(this.Keys, el.Value.(pair).K)
    }
}

```

Another way of doing this:

```go
package main

import (
    "container/list"
)

type LRUCache struct {
    capacity int
    cache    map[int]*list.Element
    list     *list.List
}

type pair struct {
    key   int
    value int
}

func Constructor(capacity int) LRUCache {
    return LRUCache{
        capacity: capacity,
        cache:    make(map[int]*list.Element),
        list:     list.New(),
    }
}

func (this *LRUCache) Get(key int) int {
    if element, ok := this.cache[key]; ok {
        this.list.MoveToFront(element)
        return element.Value.(pair).value
    }
    return -1
}

func (this *LRUCache) Put(key int, value int) {
    if element, ok := this.cache[key]; ok {
        this.list.MoveToFront(element)
        element.Value = pair{key, value}
    } else {
        if this.list.Len() >= this.capacity {
            delete(this.cache, this.list.Back().Value.(pair).key)
            this.list.Remove(this.list.Back())
        }
        this.cache[key] = this.list.PushFront(pair{key, value})
    }
}
```


