## Floyd's Tortoise and Hare (Cycle Detection) algorithm

The Floyd's Tortoise and Hare algorithm is a pointer algorithm that uses two pointers, which move at different speeds, to detect a cycle in a linked list. The algorithm is also known as the slow and fast pointers algorithm.

[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

The problem statement guarantees that there is at least one duplicate because there are n+1 integers and each integer is in the range [1, n] inclusive. This means that the array can be interpreted as a linked list where the value of each element is the index of the next node.

The time complexity is O(n) and the space complexity is O(1).

The line fast = nums[nums[fast]] is equivalent to moving the fast pointer two steps at a time. The outer nums[] corresponds to the second step. This is because each value in the array is treated as a pointer to another index in the array.

The array can be transformed into a linked list problem because each value in the array points to another index in the array, similar to how each node in a linked list points to another node. If there is a duplicate value in the array, there will be two "pointers" to the same index, creating a cycle, similar to a cycle in a linked list.

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

The array [3,1,3,4,2] can be interpreted as a linked list where the value of each element is the index of the next node. Here's how it looks:

Start at index 0, the value is 3, so go to index 3.
At index 3, the value is 4, so go to index 4.
At index 4, the value is 2, so go to index 2.
At index 2, the value is 3, so go to index 3.
At index 3, the value is 4, so go to index 4.
At index 4, the value is 2, so go to index 2.
As you can see, we're now in a cycle: 3 -> 4 -> 2 -> 3 -> 4 -> 2 -> ...

So, the duplicate number 3 is the entry point of the cycle. The Floyd's Tortoise and Hare algorithm can be used to find this entry point, which is the solution to the problem.

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

