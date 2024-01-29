[1929. Concatenation of Array](http://www.leetcode.com/problems/concatenation-of-array/)

```go
func getConcatenation(nums []int) []int {
    return append(nums, nums...)
}

func getConcatenation2(nums []int) []int {
    n := len(nums)
    res := make([]int, 2*n)
    for i := range nums {
        res[i] = nums[i]
        res[i+n] = nums[i]
    }
    return res
}
```

[1299. Replace Elements with Greatest Element on Right Side](http://www.leetcode.com/problems/replace-elements-with-greatest-element-on-right-side/)

```go  
// O(n) time, O(1) space
func replaceElements(arr []int) []int {
    n := len(arr)
    res := make([]int, n)
    res[n-1] = -1
    for i := n-2; i >= 0; i-- {
        res[i] = max(res[i+1], arr[i+1])
    }
    return res
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

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

[1672. Richest Customer Wealth](http://www.leetcode.com/problems/richest-customer-wealth/)

```go

```

[1920. Build Array from Permutation](http://www.leetcode.com/problems/build-array-from-permutation/)

```go