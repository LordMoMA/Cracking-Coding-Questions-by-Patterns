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

[143. Reorder List](https://leetcode.com/problems/reorder-list/description/)

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reorderList(head *ListNode)  {
    if head == nil || head.Next == nil {
        return
    }
    // find the middle
    fast, slow := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }

    // reverse the second part
    curr, prev := slow, (*ListNode)(nil)
    for curr != nil {
        nextTemp := curr.Next
        curr.Next = prev
        prev = curr
        curr = nextTemp
    }

    // merge first and the second
    first, second := head, prev
    for second.Next != nil {
        firstNext := first.Next
        secondNext := second.Next

        first.Next = second
        second.Next = firstNext

        first = firstNext
        second = secondNext
    }
}

```

[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{Next: head}
    fast, slow := dummy, dummy

    // Move fast n steps ahead
    for i := 0; i < n; i++ {
        if fast.Next != nil {
            fast = fast.Next
        } else {
            // If n is greater than the length of the list, just return the original list
            return head
        }
    }

    // Move fast to the end, maintaining the gap
    for fast.Next != nil {
        fast = fast.Next
        slow = slow.Next
    }

    // Skip the nth node from the end
    slow.Next = slow.Next.Next

    return dummy.Next
}
```

[141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

```go
func hasCycle(head *ListNode) bool {
    // if head == nil {
    //     return false
    // }
    fast, slow := head, head
    for fast != nil && fast.Next != nil {
        fast = fast.Next.Next
        slow = slow.Next
        if fast == slow {
            return  true
        }
    }
    return false
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

