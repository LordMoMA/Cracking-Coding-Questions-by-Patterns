## Binary Search 

The choice between for l < r and for l <= r in binary search depends on the problem you're trying to solve.

for l < r: This is used when you're trying to find a specific element in the array, and you want to narrow down the search space to a single element. In this case, the loop continues until l and r point to the same element. This is what's happening in the findMin function. The loop continues until l and r point to the minimum element.

for l <= r: This is used when you're trying to find the position of a target value in the array. In this case, the loop continues until l and r cross each other. If the target is found, the function returns the position. If the target is not found, l will be the position where the target should be inserted to maintain the sorted order of the array. This is what's happening in the search function.

So, the choice between for l < r and for l <= r depends on whether you're trying to narrow down the search space to a single element or find the position of a target value.

When `for l < r` use `r = mid` (element)
When `for l <= r` use `r = mid - 1` (index)

[162. Find Peak Element](https://leetcode.com/problems/find-peak-element/description/)

This one is different, we are looking for the index, but we used l < r instead of l <= r.

```go
func findPeakElement(nums []int) int {
    l, r := 0, len(nums)-1
    for l < r {
        mid := l + (r-l)/2
        if nums[mid] < nums[mid+1] {
            l = mid+1
        } else {
            r = mid
        }
    }
    return r
}
```

[704. Binary Search](http://leetcode.com/problems/binary-search/)

```go
func search(nums []int, target int) int {
    l, r := 0, len(nums) - 1
    for l <= r {
        mid := l + (r - l) / 2
        if nums[mid] == target {
            return mid
        }
        if nums[mid] < target {
            l = mid + 1
        } else {
            r = mid - 1
        }
    }
    return -1
}
```

[153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

```go
func findMin(nums []int) int {
    l, r := 0, len(nums) - 1
    for l < r {
    mid := l + (r - l) / 2
        if nums[mid] > nums[r] {
            l = mid + 1
        } else {
            r = mid
        }
    }
    return nums[l]
}
```

[33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)
Tik Tok Interview Question on July 2nd, 2024, interviewed by Lead Ad team, Interviewer was in China and a follow-up question related to this one was asked.
The question is find the max index of target if the array has duplicated values. for example [1,2,3,3,3] target = 3, the answer is 4, not 2 or 3.

```go
func search(nums []int, target int) int {
    l, r := 0, len(nums) - 1
    for l <= r {
        mid := l + (r - l) / 2
        if nums[mid] == target {
            return mid
        }
        if nums[l] <= nums[mid] {
             if nums[l] <= target && target < nums[mid] {
                r = mid - 1
            } else {
                l = mid + 1
            }       
        } else {
            if nums[mid] < target && target <= nums[r] {
                l = mid + 1
            } else {
                r = mid - 1
            }
        }
    }
    return -1
}
```

[4. Median of Two Sorted Arrays](http://www.leetcode.com/problems/median-of-two-sorted-arrays/)

```go
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
    if len(nums1) > len(nums2) {
        nums1, nums2 = nums2, nums1
    }

    x, y := len(nums1), len(nums2)
    start, end := 0, x

    for start <= end {
        partitionX := (start + end) / 2
        partitionY := (x + y + 1) / 2 - partitionX

        maxLeftX := getMax(nums1, partitionX)
        minRightX := getMin(nums1, partitionX)

        maxLeftY := getMax(nums2, partitionY)
        minRightY := getMin(nums2, partitionY)

        if maxLeftX <= minRightY && maxLeftY <= minRightX {
            if (x+y)%2 == 0 {
                return float64(max(maxLeftX, maxLeftY)+min(minRightX, minRightY)) / 2.0
            } else {
                return float64(max(maxLeftX, maxLeftY))
            }
        } else if maxLeftX > minRightY {
            end = partitionX - 1
        } else {
            start = partitionX + 1
        }
    }

    return -1
}

func getMax(nums []int, partition int) int {
    if partition == 0 {
        return math.MinInt32
    }
    return nums[partition-1]
}

func getMin(nums []int, partition int) int {
    if partition == len(nums) {
        return math.MaxInt32
    }
    return nums[partition]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

Solved in DP, could also be solved in Binary Search Solution

This solution has a time complexity of O(n log n) and a space complexity of O(n), where n is the length of the input slice nums.

if nums = [10, 9, 2, 5, 3, 7, 101, 18], the code will produce tails = [2, 3, 7, 18] and return 4

let's break down how this code works with an example. Suppose we have nums = [10, 9, 2, 5, 3, 7, 101, 18] and we're iterating through nums in the lengthOfLIS function.

num = 10: tails is currently empty. sort.Search returns 0 because there's no element in tails that is greater than or equal to 10. Since 0 == len(tails), we append 10 to tails. Now, tails = [10].

num = 9: sort.Search returns 0 because 10 (the first element in tails) is greater than 9. Since 0 != len(tails), we replace tails[0] with 9. Now, tails = [9].

num = 2: sort.Search returns 0 because 9 is greater than 2. We replace tails[0] with 2. Now, tails = [2].

num = 5: sort.Search returns 1 because there's no element in tails that is greater than or equal to 5. We append 5 to tails. Now, tails = [2, 5].

num = 3: sort.Search returns 1 because 5 is greater than 3. We replace tails[1] with 3. Now, tails = [2, 3].

num = 7: sort.Search returns 2 because there's no element in tails that is greater than or equal to 7. We append 7 to tails. Now, tails = [2, 3, 7].

num = 101: sort.Search returns 3 because there's no element in tails that is greater than or equal to 101. We append 101 to tails. Now, tails = [2, 3, 7, 101].

num = 18: sort.Search returns 3 because 101 is greater than 18. We replace tails[3] with 18. Now, tails = [2, 3, 7, 18].

At the end of the process, tails contains the smallest tail elements of all increasing subsequences of lengths 1, 2, 3, 4. The length of tails is 4, which is the length of the longest increasing subsequence in nums.

```go
import "sort"

func lengthOfLIS(nums []int) int {
    tails := []int{}
    for _, num := range nums {
        i := sort.Search(len(tails), func(i int) bool { return tails[i] >= num })
        if i == len(tails) {
            tails = append(tails, num)
        } else {
            tails[i] = num
        }
    }
    return len(tails)
}
```

```go
func lengthOfLIS(nums []int) int {
    tails := []int{}
    for _, num := range nums {
        left, right := 0, len(tails)
        for left < right {
            mid := left + (right-left)/2
            if tails[mid] < num {
                left = mid + 1
            } else {
                right = mid
            }
        }
        i := left
        if i == len(tails) {
            tails = append(tails, num)
        } else {
            tails[i] = num
        }
    }
    return len(tails)
}
```

[74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/description/)

To convert a 2D index (row, column) into a 1D index in a matrix, you can use the formula index = row * numberOfColumns + column.

row * numberOfColumns gives the index of the first element of the row in the flattened 1D array.

+ column then offsets this index to the right column.

For example, in a 3x3 matrix, the 2D index (1, 2) (second row, third column) would be converted to the 1D index 1 * 3 + 2 = 5.

```go
func convert2DTo1D(row int, col int, cols int) int {
    return row * cols + col
}
```

1D -> 2D:

```go
func convert1DTo2D(index int, cols int) (int, int) {
    row := index / cols
    col := index % cols
    return row, col
}
```

```go
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    rows, cols := len(matrix), len(matrix[0])
    l, r := 0, rows*cols-1
    for l <= r {
        mid := l + (r-l)/2
        midValue := matrix[mid/cols][mid%cols]
        if midValue == target {
            return true
        } else if midValue < target {
            l = mid + 1
        } else {
            r = mid - 1
        }
    }
    return false
}
```

[875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/description/)

Koko can decide her bananas-per-hour eating speed of k

If l := 0, it will cause "runtime error: integer divide by zero".

The panic "runtime error: integer divide by zero" is caused by the division operation in the hours function. If l is initialized to 0 in the minEatingSpeed function, then k in the hours function can be 0 when l and r are both 0 and k := l + (r-l)/2 is calculated.

The time complexity of the minEatingSpeed function is O(N log M), where N is the number of piles and M is the maximum number of bananas in a pile. This is because in the worst case, we perform a binary search over the range [1, M] (which takes O(log M) time) and for each mid value, we iterate over all the piles to calculate the hours needed (which takes O(N) time).

The space complexity of the minEatingSpeed function is O(1), which means the space required by the algorithm is constant, regardless of the number of piles. This is because we only use a fixed amount of space to store the variables l, r, k, and the results of the hours and max functions, and this does not change with the input size.

```go
func minEatingSpeed(piles []int, H int) int {
    l, r := 1, 1
    for _, pile := range piles {
        r = max(r, pile)
    }
    for l < r {
        k := l + (r-l)/2
        // This is because if the hours required at the current speed mid are more than h, we need to increase the speed, not decrease it.
        if hours(piles, k) > H {
            l = k + 1
        } else {
            r = k
        }
    }
    return l
}

func hours(piles []int, k int) int {
    res := 0
    for _, pile := range piles {
        res += (pile + k - 1) / k
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

Math Tricks:

Adding K to the pile size before dividing by K would indeed increase the numerator to the next multiple of K, but it would also overestimate the number of hours needed.

Consider a case where pile is 10 and K is 3. The actual number of hours needed would be 4 (3 hours for the first 9 bananas and 1 more hour for the last banana).

If you calculate (pile + K) / K, it would be (10 + 3) / 3 = 13 / 3 = 4.33, which rounds down to 4 in integer division. This is correct in this case.

However, if pile is 9 and K is 3, the actual number of hours needed would be 3. But (pile + K) / K would be (9 + 3) / 3 = 12 / 3 = 4, which overestimates the number of hours.

That's why we use (pile + K - 1) / K. It ensures that the result is rounded up to the nearest integer without overestimating the number of hours. For pile = 9 and K = 3, (pile + K - 1) / K would be (9 + 3 - 1) / 3 = 11 / 3 = 3.67, which rounds down to 3 in integer division, correctly estimating the number of hours.

[981. Time Based Key-Value Store](http://www.leetcode.com/problems/time-based-key-value-store/)

The sort.Search function in Go is used to perform a binary search on a sorted slice. It uses a "half-interval search" algorithm, also known as binary search, to find the smallest index i in [0, n) at which f(i) is true, given the function f is a "monotone predicate", which means its truth value is consistent as i increases.

sort.Search in Go is an implementation of binary search on a predicate.

```go
x := []int{1, 3, 5, 8, 9}
i := sort.Search(len(x), func(i int) bool { return x[i] >= 5 })
fmt.Println(i)  // Output: 2
```

Note:

```go
x := []int{1, 3, 5, 8, 9}
i := sort.Search(len(x), func(i int) bool { return x[i] < 5 })
fmt.Println(i)  // Output: 2
```

the above two are the same, because the predicate is monotone.

Note:

If below format is adopted, there should be a comma!

```go
func Constructor() TimeMap {
    return TimeMap{
        m: make(map[string][]pair),
    }
}
```

What does TimeMap looks like:
```go
tm := TimeMap{
    m: map[string][]pair{
        "key1": {
            {timestamp: 1, value: "value1"},
            {timestamp: 2, value: "value2"},
        },
        "key2": {
            {timestamp: 3, value: "value3"},
            {timestamp: 4, value: "value4"},
        },
    },
}
```

Solution:

```go
type TimeMap struct {
    m map[string][]pair
}

type pair struct {
    timestamp int
    value     string
}

/** Initialize your data structure here. */
func Constructor() TimeMap {
    return TimeMap{m: make(map[string][]pair)}
}

func (this *TimeMap) Set(key string, value string, timestamp int) {
    this.m[key] = append(this.m[key], pair{timestamp, value})
}

// time O(log N) space O(1)
func (this *TimeMap) Get(key string, timestamp int) string {
    pairs := this.m[key]
    // This will return the index of the first pair whose timestamp is greater than the given timestamp. If there is no such pair, it will return the length of the pairs slice.
    i := sort.Search(len(pairs), func(i int) bool { return pairs[i].timestamp > timestamp })
    if i > 0 {
        return pairs[i-1].value
    }
    return ""
}
```

Implement Get manually:

```go
func (this *TimeMap) Get2(key string, timestamp int) string {
    pairs := this.m[key]
    left, right := 0, len(pairs)
    for left < right {
        mid := left + (right - left) / 2
        if pairs[mid].timestamp <= timestamp {
            left = mid + 1
        } else {
            right = mid
        }
    }
    if left > 0 {
        return pairs[left-1].value
    }
    return ""
}
```

The reason l, r := 0, len(pairs) is used instead of l, r := 0, len(pairs)-1 is because of the specific binary search variant being used. This variant is sometimes called "binary search for the first true" or "binary search on a predicate".

In this variant, the search space is [l, r) - that is, it includes l but excludes r. The goal is to find the smallest l such that the predicate is true, or if there is no such l, return r.

This is why the loop condition is l < r instead of l <= r, and why r is set to mid instead of mid - 1 when the predicate is true. The r value is never a valid answer, so it's safe to set r to len(pairs) initially.

This variant of binary search is particularly useful when the search space is infinite or not clearly defined, as it only requires the ability to compare elements, not to index them.

A predicate is a function that returns a boolean value. In the context of a binary search, a predicate is typically a function that takes an index or a value from the array and returns true if the desired condition is met and false otherwise.

"Binary search on a predicate" means performing a binary search based on the result of a predicate function. Instead of searching for a specific value in a sorted array, you're searching for the first or last index where the predicate function returns true.

For example, if you have a sorted array of integers and a predicate function that returns true for values greater than or equal to 10, a binary search on this predicate would find the first index in the array where the value is 10 or more.

This kind of binary search is useful when the condition you're searching for can't be expressed simply as "find this value". It's also useful when the array is infinite or not clearly defined, as the predicate function can be used to determine the bounds of the search.

sort.Search in Go is an implementation of binary search on a predicate.

Solution 3:

```go
type TimeMap struct {
	store map[string][]ValStamp // key : list of [val, timestamp]
}

type ValStamp struct {
	Val  string
	Time int
}

func Constructor() TimeMap {
	return TimeMap{store: make(map[string][]ValStamp)}
}

func (this *TimeMap) Set(key string, value string, timestamp int) {
	if _, ok := this.store[key]; !ok {
		this.store[key] = make([]ValStamp, 0)
	}
	this.store[key] = append(this.store[key], ValStamp{value, timestamp})
}

func (this *TimeMap) Get(key string, timestamp int) string {
	var res string
	var values []ValStamp
	if _, ok := this.store[key]; ok {
		values = this.store[key]
	}
	l, r := 0, len(values)-1
	for l <= r {
		mid := l + (r-l)/2
		if values[mid].Time <= timestamp {
			res = values[mid].Val
			l = mid + 1
		} else {
			r = mid - 1
		}
	}
	return res
}
```
