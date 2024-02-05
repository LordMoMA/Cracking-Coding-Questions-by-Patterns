## Floyd's Tortoise and Hare (Cycle Detection) algorithm

The Floyd's Tortoise and Hare algorithm is a pointer algorithm that uses two pointers, which move at different speeds, to detect a cycle in a linked list. The algorithm is also known as the slow and fast pointers algorithm.

[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

```go
func findDuplicate(nums []int) int {
    slow, fast := nums[0], nums[nums[0]]
    for slow != fast {
        slow = nums[slow]
        fast = nums[nums[fast]]
    }
    slow = 0
    for slow != fast {
        slow = nums[slow]
        fast = nums[fast]
    }
    return slow
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

