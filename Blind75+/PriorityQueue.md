[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

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


[692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
[703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
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