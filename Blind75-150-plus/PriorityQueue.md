##  Priority Queue

In Go, the sort.Interface requires three methods: Len, Less, and Swap.

The heap.Interface includes the sort.Interface and adds two more methods: Push and Pop.

When you're implementing a priority queue using the container/heap package in Go, you need to define these five methods.

In Go, the container/heap package uses the sort.Interface as part of its heap.Interface. This means that when you implement a type that satisfies the heap.Interface, it must also satisfy the sort.Interface because the heap.Interface embeds the sort.Interface.

When you import container/heap, you're implicitly making use of the sort.Interface without directly importing sort. The heap functions internally call the Len, Less, and Swap methods of your type, which are required by the sort.Interface.



[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

The heap data structure in Go does not provide direct access to its elements. It only provides access to the top element (the minimum or maximum, depending on whether it's a min-heap or max-heap) through the heap.Pop() function. This is because heaps are binary trees that are stored in arrays for efficiency, and the elements are not sorted in a way that allows direct access.

```go
func topKFrequent(nums []int, k int) []int {
    countMap := map[int]int{}
    for _, num := range nums {
        countMap[num]++
    }

    m := &minHeap{}
    heap.Init(m)
    for num, count := range countMap {
        heap.Push(m, pair{num, count})
        if m.Len() > k {
            heap.Pop(m)
        }
    }

    res := make([]int, k)
    for i := 0; i < k; i++ {
        res[k-i-1] = heap.Pop(m).(pair).num
    }
    return res
}

type pair struct {
    num int
    count int
}

type minHeap []pair

func (m minHeap) Len() int {
    return len(m)
}

func (m minHeap) Less(i, j int) bool {
    return m[i].count < m[j].count
}

func (m minHeap) Swap(i, j int) {
    m[i], m[j] = m[j], m[i]
}

func (m *minHeap) Push(x interface{}) {
    *m = append(*m, x.(pair))
}

func (m *minHeap) Pop() interface{} {
    old := *m
    n := len(old)
    x := old[n - 1]
    *m = old[:n - 1]
    return x
}
```

[23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description/)

The dummy node is a common technique used in linked list problems to simplify edge cases, particularly where the head of the list might change.

In removeNthFromEnd, the dummy node is initialized with Next pointing to head because the function needs to return the head of the modified list, which might be different from the original head if the nth node from the end happens to be the head of the list. By having dummy.Next point to head, we can always return dummy.Next as the head of the modified list.

In mergeKLists, the dummy node is initialized with Next as nil because it serves as a placeholder for the head of the new merged list. As we merge the lists, we attach nodes to dummy.Next and then move dummy forward. At the end, dummy.Next will be the head of the merged list.

```go
import "container/heap"

type ListNodeHeap []*ListNode

func (h ListNodeHeap) Len() int           { return len(h) }
func (h ListNodeHeap) Less(i, j int) bool { return h[i].Val < h[j].Val }
func (h ListNodeHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *ListNodeHeap) Push(x interface{}) {
    *h = append(*h, x.(*ListNode))
}

func (h *ListNodeHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func mergeKLists(lists []*ListNode) *ListNode {
    h := &ListNodeHeap{}
    heap.Init(h)

    for _, node := range lists {
        if node != nil {
            heap.Push(h, node)
        }
    }

    dummy := &ListNode{}
    current := dummy

    for h.Len() > 0 {
        minNode := heap.Pop(h).(*ListNode)
        current.Next = minNode
        current = current.Next

        if minNode.Next != nil {
            heap.Push(h, minNode.Next)
        }
    }

    return dummy.Next
}
```

we return dummy.Next because that's the head of the merged list. If we returned dummy, we would be returning the dummy node we created, not the actual list.

[630. Course Schedule III](https://leetcode.com/problems/course-schedule-iii/description/)

```go
import (
    "container/heap"
    "sort"
)

func scheduleCourse(courses [][]int) int {
    sort.Slice(courses, func(i, j int) bool {
        return courses[i][1] < courses[j][1]
    })

    maxHeap := &IntHeap{}
    heap.Init(maxHeap)
    time := 0

    for _, course := range courses {
        time += course[0]
        heap.Push(maxHeap, course[0])
        if time > course[1] {
            time -= heap.Pop(maxHeap).(int)
        }
    }

    return maxHeap.Len()
}

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

[703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

```go
import "container/heap"

type KthLargest struct {
    k int
    minHeap *IntHeap
}

func Constructor(k int, nums []int) KthLargest {
    h := &IntHeap{}
    heap.Init(h)
    for _, num := range nums {
        heap.Push(h, num)
        if h.Len() > k {
            heap.Pop(h)
        }
    }
    return KthLargest{k, h}
}

func (this *KthLargest) Add(val int) int {
    heap.Push(this.minHeap, val)
    if this.minHeap.Len() > this.k {
        heap.Pop(this.minHeap)
    }
    return (*this.minHeap)[0]
}

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

[1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)

time complexity of O(n log n) and a space complexity of O(n).

The function first pushes all elements from the stones array into a heap. Each push operation takes O(log n) time, where n is the number of elements in the heap. So, pushing all n elements into the heap takes O(n log n) time.
Then, in the worst case, the function performs n-1 pop operations and up to n-1 push operations (when all stones have different weights). Each of these operations takes O(log n) time, so this part of the function also takes O(n log n) time.
Therefore, the total time complexity is O(n log n) + O(n log n) = O(n log n).

```go
import "container/heap"

func lastStoneWeight(stones []int) int {
    h := &IntHeap{}
    heap.Init(h)
    for _, stone := range stones {
        heap.Push(h, stone)
    }
    for h.Len() > 1 {
        y := heap.Pop(h).(int)
        x := heap.Pop(h).(int)
        if x != y {
            heap.Push(h, y-x)
        }
    }
    if h.Len() == 0 {
        return 0
    }
    return heap.Pop(h).(int)
}

type IntHeap []int
```

In the lastStoneWeight function, the result of heap.Pop(h) is being assigned to a variable directly. The heap.Pop function returns a value of type interface{}, which is a placeholder for any type in Go. When you assign this to a variable, Go doesn't know what the actual type of the value is, so you need to use a type assertion to tell Go that the value is an int.

In the KthLargest struct methods, the result of heap.Pop(this.minHeap) is not being assigned to a variable. Instead, it's being discarded because the code doesn't need the value that was removed from the heap. Therefore, there's no need for a type assertion in this case.



```go
import "container/heap"

func kClosest(points [][]int, k int) [][]int {
    h := &IntHeap{}
    heap.Init(h)
    for _, point := range points {
        heap.Push(h, point)
        if h.Len() > k {
            heap.Pop(h)
        }
    }
    res := make([][]int, k)
    for i := k - 1; i >= 0; i-- {
        res[i] = heap.Pop(h).([]int)
    }
    return res
}

type IntHeap [][]int //maxHeap

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i][0]*h[i][0]+h[i][1]*h[i][1] > h[j][0]*h[j][0]+h[j][1]*h[j][1] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
    *h = append(*h, x.([]int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

```go

func findKthLargest(nums []int, k int) int {
    h := &IntHeap{}
    heap.Init(h)
    for _, num := range nums {
        heap.Push(h, num)
        if h.Len() > k {
            heap.Pop(h)
        }
    }
    return heap.Pop(h).(int)
}

type IntHeap []int
```

[621. Task Scheduler](https://leetcode.com/problems/task-scheduler/)

```go
import "container/heap"

func leastInterval(tasks []byte, n int) int {
    count := map[byte]int{}
    for _, task := range tasks {
        count[task]++
    }

    h := &IntHeap{}
    heap.Init(h)
    for _, c := range count {
        heap.Push(h, c)
    }

    cycles := 0
    for h.Len() > 0 {
        temp := []int{}
        for i := 0; i <= n; i++ {
            if h.Len() > 0 {
                temp = append(temp, heap.Pop(h).(int))
            }
        }
        for _, t := range temp {
            if t-1 > 0 {
                heap.Push(h, t-1)
            }
        }
        if h.Len() == 0 {
            cycles += len(temp)
        } else {
            cycles += n + 1
        }
    }

    return cycles
}

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] > h[j] } //maxheap
```

[355. Design Twitter](https://leetcode.com/problems/design-twitter/)

The follows map[int]map[int]struct{} is a map of maps in Go, where the inner map is essentially a set. Here's an example of what it might look like for a simple Twitter-like system:

```go
follows := map[int]map[int]struct{}{
    1: {
        2: struct{}{},
        3: struct{}{},
    },
    2: {
        1: struct{}{},
    },
    3: {
        2: struct{}{},
    },
}

tweets := make(map[int][]tweet)

tweets[1] = []tweet{
    {id: 101, text: "Hello, world!"},
    {id: 102, text: "I love programming."},
}

follows := make(map[int]map[int]struct{})

follows[1] = make(map[int]struct{})
follows[1][2] = struct{}{}
```

follows[1][2] = struct{}{} could represent that user 1 follows user 2. The empty struct is used because it doesn't take any additional space, making it a memory-efficient way to represent a set in Go.

The empty struct is being used as a placeholder value in the map to represent a set. The keys of the map represent the elements of the set, and the values are irrelevant, so an empty struct is used as a placeholder because it doesn't take up any additional memory.

```go
import "container/heap"

type Twitter struct {
    time int
    tweets map[int][]tweet
    follows map[int]map[int]struct{}
}

type tweet struct {
    id int
    time int
}
// time: O(1) Space O(n+m) N is the number of users and M is the number of tweets
func Constructor() Twitter {
    return Twitter{
        tweets: make(map[int][]tweet),
        follows: make(map[int]map[int]struct{}),
    }
}

func (t *Twitter) PostTweet(userId int, tweetId int) {
    t.tweets[userId] = append(t.tweets[userId], tweet{tweetId, t.time})
    t.time++
}

// time O(N log N) Space O(N) We're storing all the tweets in a heap.
func (t *Twitter) GetNewsFeed(userId int) []int {
    h := &IntHeap{}
    heap.Init(h)
    if tweets, ok := t.tweets[userId]; ok {
        for i := len(tweets) - 1; i >= 0 && i >= len(tweets)-10; i-- {
            heap.Push(h, tweets[i])
        }
    }
    if f, ok := t.follows[userId]; ok {
        for user := range f {
            if tweets, ok := t.tweets[user]; ok {
                for i := len(tweets) - 1; i >= 0 && i >= len(tweets)-10; i-- {
                    heap.Push(h, tweets[i])
                }
            }
        }
    }
    res := []int{}
    for h.Len() > 0 && len(res) < 10 {
        res = append(res, heap.Pop(h).(tweet).id)
    }
    return res
}

func (t *Twitter) Follow(followerId int, followeeId int) {
    if _, ok := t.follows[followerId]; !ok {
        t.follows[followerId] = make(map[int]struct{})
    }
    t.follows[followerId][followeeId] = struct{}{}
}

func (t *Twitter) Unfollow(followerId int, followeeId int) {
    if f, ok := t.follows[followerId]; ok {
        delete(f, followeeId)
    }
}

type IntHeap []tweet

func (h IntHeap) Less(i, j int) bool { return h[i].time > h[j].time } //maxheap
```
[1985. Find the Kth Largest Integer in the Array](https://leetcode.com/problems/find-the-kth-largest-integer-in-the-array/description/)

time complexity: O(n log k)

Here's why:

The loop that iterates over the input array has a time complexity of O(n), as it performs a constant amount of work for each element in the array.

Inside the loop, the heap.Push and heap.Pop operations each have a time complexity of O(log k), as they need to maintain the heap property in a heap of size k. These operations are performed at most once for each element in the array.

Therefore, the overall time complexity is O(n log k), as the loop and the heap operations are combined in a nested manner.

The space complexity of the solution is O(k), as the heap used to store the k largest numbers requires k space.


### 🥶 Be careful about this problem: 

If we directly work with int in this problem we will get:

pass test: output: "9223372036854775807", expected: "38272299275037314530" when k = 3

The issue is that the int type in Go has a maximum value of 9223372036854775807 (for 64-bit systems), and the number 38272299275037314530 is larger than this maximum value.

so below tradional solution did not work:

```go
func kthLargestNumber(nums []string, k int) string {
    minHeap := &minHeap{}
    heap.Init(minHeap)

    for _, num := range nums {
        cur, _ := strconv.Atoi(num)
        heap.Push(minHeap, cur)
        if minHeap.Len() > k {
            heap.Pop(minHeap)
        }
    }
    return strconv.Itoa(heap.Pop(minHeap).(int))
}

type minHeap []int

func (m minHeap) Len() int { return len(m)}
func (m minHeap) Swap(i, j int)  { m[i], m[j] = m[j], m[i]}
func (m minHeap) Less(i, j int) bool { return m[i] < m[j]}

func (m *minHeap) Push(x interface{}) {
    *m = append(*m, x.(int))
}

func (m *minHeap) Pop() interface{} {
    old := *m
    n := len(old)
    x := old[n-1]
    *m = old[:n-1]
    return x 
}
```

To solve this problem, you can use the big.Int type from the math/big package, which can handle arbitrarily large integers. However, the container/heap package and the comparison operators (<, >, etc.) do not work with big.Int directly.

A workaround is to keep the numbers as strings and compare them as strings.

And we should know that we can compare strings using the standard comparison operators: ==, !=, <, <=, >, and >=. These operators compare strings lexicographically, character by character, based on the Unicode values of the characters.

see an example of how string comparison works:

```js
console.log("10" < "2");  // true (lexicographical comparison)
console.log(parseInt("10") < parseInt("2"));  // false (numerical comparison)
```
consider the strings "10" and "2". Lexicographically, "10" is less than "2" because '1' (the first character of "10") is less than '2'. However, numerically, 10 is greater than 2.

```go
package main

import (
    "container/heap"
    "fmt"
)

type minHeap []string

func (m minHeap) Len() int { return len(m) }
func (m minHeap) Swap(i, j int)  { m[i], m[j] = m[j], m[i] }
func (m minHeap) Less(i, j int) bool { // ❤️  this is where we need to be careful
    if len(m[i]) == len(m[j]) {
        return m[i] < m[j]
    }
    return len(m[i]) < len(m[j])
}

func (m *minHeap) Push(x interface{}) {
    *m = append(*m, x.(string))
}

func (m *minHeap) Pop() interface{} {
    old := *m
    n := len(old)
    x := old[n-1]
    *m = old[:n-1]
    return x 
}

func kthLargestNumber(nums []string, k int) string {
    minHeap := &minHeap{}
    heap.Init(minHeap)

    for _, num := range nums {
        heap.Push(minHeap, num)
        if minHeap.Len() > k {
            heap.Pop(minHeap)
        }
    }
    return heap.Pop(minHeap).(string)
}

```

[692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
[767. Reorganize String](https://leetcode.com/problems/reorganize-string/)
[973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

[1054. Distant Barcodes](https://leetcode.com/problems/distant-barcodes/)



[1054. Distant Barcodes](https://leetcode.com/problems/distant-barcodes/)
[1122. Relative Sort Array](https://leetcode.com/problems/relative-sort-array/)
[1167. Minimum Cost to Connect Sticks](https://leetcode.com/problems/minimum-cost-to-connect-sticks/)
[1337. The K Weakest Rows in a Matrix](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/)
[1354. Construct Target Array With Multiple Sums](https://leetcode.com/problems/construct-target-array-with-multiple-sums/)
[1387. Sort Integers by The Power Value](https://leetcode.com/problems/sort-integers-by-the-power-value/)
[1429. First Unique Number](https://leetcode.com/problems/first-unique-number/)
[1481. Least Number of Unique Integers after K Removals](https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/)
[1488. Avoid Flood in The City](https://leetcode.com/problems/avoid-flood-in-the-city/)
[1675. Minimize Deviation in Array](https://leetcode.com/problems/minimize-deviation-in-array/)
[1738. Find Kth Largest XOR Coordinate Value](https://leetcode.com/problems/find-kth-largest-xor-coordinate-value/)
[1792. Maximum Average Pass Ratio](https://leetcode.com/problems/maximum-average-pass-ratio/)
[1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)
[1851. Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)
[1882. Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)
[1883. Minimum Skips to Arrive at Meeting On Time](https://leetcode.com/problems/minimum-skips)

[347. Top





[767. Reorganize String](https://leetcode.com/problems/reorganize-string/)
[973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)
[1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
[1054. Distant Barcodes](https://leetcode.com/problems/distant-barcodes/)
[1122. Relative Sort Array](https://leetcode.com/problems/relative-sort-array/)
[1167. Minimum Cost to Connect Sticks](https://leetcode.com/problems/minimum-cost-to-connect-sticks/)
[1337. The K Weakest Rows in a Matrix](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/)
[1354. Construct Target Array With Multiple Sums](https://leetcode.com/problems/construct-target-array-with-multiple-sums/)
[1387. Sort Integers by The Power Value](https://leetcode.com/problems/sort-integers-by-the-power-value/)
[1429. First Unique Number](https://leetcode.com/problems/first-unique-number/)
[1481. Least Number of Unique Integers after K Removals](https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/)
[1488. Avoid Flood in The City](https://leetcode.com/problems/avoid-flood-in-the-city/)
[1675. Minimize Deviation in Array](https://leetcode.com/problems/minimize-deviation-in-array/)
[1738. Find Kth Largest XOR Coordinate Value](https://leetcode.com/problems/find-kth-largest-xor-coordinate-value/)
[1792. Maximum Average Pass Ratio](https://leetcode.com/problems/maximum-average-pass-ratio/)
[1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)
[1851. Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)
[1882. Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)
[1883. Minimum Skips to Arrive at Meeting On Time](https://leetcode.com/problems/minimum-skips)
