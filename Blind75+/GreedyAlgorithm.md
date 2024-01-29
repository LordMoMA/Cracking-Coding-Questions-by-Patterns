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