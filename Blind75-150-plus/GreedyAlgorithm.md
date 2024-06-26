## Greedy Algorithms

Greedy algorithms make the locally optimal choice at each step in the hope that these local choices will lead to a global optimum. They are typically used for optimization problems.

[121. Best Time to Buy and Sell Stock](http://www.leetcode.com/problems/best-time-to-buy-and-sell-stock/)

```go
func maxProfit(prices []int) int {
    minPrice := math.MaxInt64
    maxProfit := 0

    for _, price := range prices {
        if price < minPrice {
            minPrice = price
        } else if price - minPrice > maxProfit {
            maxProfit = price - minPrice
        }
    }

    return maxProfit
}
```

[122. Best Time to Buy and Sell Stock II](http://www.leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)



[55. Jump Game](https://leetcode.com/problems/jump-game/description/)

This solution has a time complexity of O(n) and a space complexity of O(1), where n is the length of the input array nums. It's more efficient than the traditional DP solution.

```go
func canJump(nums []int) bool {
    lastPos := len(nums) - 1
    for i := len(nums) - 1; i >= 0; i-- {
        if i + nums[i] >= lastPos {
            lastPos = i
        }
    }
    return lastPos == 0
}
```

The reason we work backwards is because it simplifies the problem.

If we start from the beginning, we would have to keep track of all possible positions we could reach from the current position and then check if any of them can reach the end. This could potentially involve tracking multiple "paths" through the array, which can complicate the algorithm.

On the other hand, if we start from the end, we only need to keep track of one thing: the furthest position from the end that we know we can reach (i.e., lastPos). At each position, we just check if we can reach lastPos from there. If we can, we update lastPos to be the current position. This simplifies the problem to only tracking a single "path" through the array.

Traditional DP:

This solution has a time complexity of O(n^2) and a space complexity of O(n)

```go
func canJump(nums []int) bool {
    n := len(nums)
    dp := make([]bool, n)
    dp[n-1] = true // we can always reach the end from the end

    for i := n - 2; i >= 0; i-- {
        furthestJump := min(i + nums[i], n - 1)
        for j := i + 1; j <= furthestJump; j++ {
            if dp[j] == true {
                dp[i] = true
                break
            }
        }
    }

    return dp[0]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

We initialize dp[n-1] to true because we can always reach the end from the end. Then we iterate backwards over the array, and for each index i, we check if there's any index j that we can jump to from i such that dp[j] is true. If there is, we set dp[i] to true and break the loop.

## Interval Problems

In general, if you're trying to merge or combine intervals in some way, it's often helpful to sort by start time so you can process the intervals in the order they begin.

If you're trying to select or remove intervals, it's often helpful to sort by end time so you can make decisions based on when intervals end.

[57. Insert Interval](https://leetcode.com/problems/insert-interval/description/)

```go
func insert(intervals [][]int, newInterval []int) [][]int {
    var result [][]int
    i := 0

    // Add to result all the intervals ending before newInterval starts.
    for i < len(intervals) && intervals[i][1] < newInterval[0] {
        result = append(result, intervals[i])
        i++
    }

    // Merge all overlapping intervals to one considering newInterval.
    for i < len(intervals) && intervals[i][0] <= newInterval[1] {
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i++
    }

    // Add the union of intervals we got.
    result = append(result, newInterval)

    // Add all the rest.
    for i < len(intervals) {
        result = append(result, intervals[i])
        i++
    }

    return result
}
```

[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)

time : O(n log n) space: O(n)

```go
import "sort"

func merge(intervals [][]int) [][]int {
    if len(intervals) == 0 {
        return intervals
    }

    // Sort the intervals by start time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    result := [][]int{intervals[0]}

    for _, interval := range intervals[1:] {
        last := result[len(result)-1]

        if interval[0] <= last[1] {
            // If the current interval overlaps with the last interval in the result,
            // merge them by updating the end time of the last interval.
            last[1] = max(last[1], interval[1])
        } else {
            // Otherwise, add the current interval to the result.
            result = append(result, interval)
        }
    }

    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)

We sort by end time because we want to select the interval that ends the earliest (to leave as much room as possible for the remaining intervals), which requires processing the intervals in the order they end.

```go
import "sort"

func eraseOverlapIntervals(intervals [][]int) int {
    if len(intervals) == 0 {
        return 0
    }

    // Sort the intervals by end time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][1] < intervals[j][1]
    })

    end, count := intervals[0][1], 1

    for _, interval := range intervals[1:] {
        if interval[0] >= end {
            // If the current interval does not overlap with the last one,
            // update the end time and increment the count.
            end = interval[1]
            count++
        }
    }

    // The minimum number of intervals to remove is the total number of intervals minus the maximum number of non-overlapping intervals.
    return len(intervals) - count
}
```

The variable count is initialized as 1 because we automatically include the first interval (after sorting) in our count of non-overlapping intervals. This is because the first interval will always be part of the solution, as there are no previous intervals that it can overlap with. We then iterate over the remaining intervals, incrementing count each time we find a non-overlapping interval.

[252. Meeting Rooms](http://www.leetcode.com/problems/meeting-rooms/)

The problem "252. Meeting Rooms" asks if a person could attend all meetings. Given an array of meeting time intervals where intervals[i] = [starti, endi], the task is to determine if a person could attend all meetings.

If we sorted by end time, we would still need to check for overlaps, but the check would be more complicated because an interval could potentially overlap with multiple previous intervals, not just the immediately preceding one.

```go
import "sort"

func canAttendMeetings(intervals [][]int) bool {
    // Sort the intervals by start time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    // Check for any overlapping intervals
    for i := 1; i < len(intervals); i++ {
        if intervals[i][0] < intervals[i-1][1] {
            // If the start time of the current interval is earlier than the end time of the previous interval,
            // then the person cannot attend all meetings.
            return false
        }
    }

    // If no overlapping intervals were found, then the person can attend all meetings.
    return true
}
```

[253. Meeting Rooms II](http://www.leetcode.com/problems/meeting-rooms-ii/)

```go

```

[253. Meeting Rooms II](http://www.leetcode.com/problems/meeting-rooms-ii/)

Min heap for end times:

The min heap is used to track the end times of the meetings currently being held. The meeting that ends the earliest is always at the top of the min heap. This is important because when a new meeting is scheduled to start, we need to check if there's a meeting that ends before or at the same time. If such a meeting exists, we can use the same room for the new meeting. If not, we need to allocate a new room.

```go
import (
    "container/heap"
    "sort"
)

func minMeetingRooms(intervals [][]int) int {
    if len(intervals) == 0 {
        return 0
    }

    // Sort the intervals by start time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    // Initialize a min heap to store the end time of intervals
    minHeap := &IntHeap{intervals[0][1]}
    heap.Init(minHeap)

    // Iterate over the intervals
    for _, interval := range intervals[1:] {
        // If the current interval's start time is greater than or equal to the earliest end time,
        // then we can reuse the room
        if interval[0] >= (*minHeap)[0] {
            heap.Pop(minHeap)
        }

        // Push the end time of the current interval into the min heap
        heap.Push(minHeap, interval[1])
    }

    // The size of the min heap is the minimum number of rooms required
    return minHeap.Len()
}

type IntHeap []int

func (h IntHeap) Len() int { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

[678. Valid Parenthesis String](https://leetcode.com/problems/valid-parenthesis-string/description/)

In the context of the lo variable, we treat the asterisk as a closing parenthesis. This is because we're considering the "worst-case" scenario where we have the minimum number of unmatched opening parentheses.

In this worst-case scenario, we want to match as many parentheses as possible to minimize the number of unmatched opening parentheses. So, when we see an asterisk, we consider it as a closing parenthesis ')' (if there's an unmatched opening parenthesis), or as an empty string (if there's no unmatched opening parenthesis), and we decrement lo.

However, it's important to note that this is just one possible way to interpret the asterisk. In the context of the hi variable, we treat the asterisk as an opening parenthesis, because we're considering the "best-case" scenario where we have the maximum number of unmatched opening parentheses.

This dual interpretation of the asterisk allows us to keep track of the full range of possible numbers of unmatched opening parentheses, which helps us determine whether the string can be made valid.

```go
func checkValidString(s string) bool {
    lo, hi := 0, 0
    for _, c := range s {
        if c == '(' {
            lo++
        } else {
            lo--
        }
        if c != ')' {
            hi++
        } else {
            hi--
        }
        if hi < 0 {
            break
        }
        lo = max(lo, 0)
    }
    return lo == 0
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```
[122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

```go
func maxProfit(prices []int) int {
    profit := 0
    for i := 1; i < len(prices); i++ {
        if prices[i] > prices[i-1]{
            profit += prices[i] - prices[i-1]
        }
    }
    return profit
}
```
