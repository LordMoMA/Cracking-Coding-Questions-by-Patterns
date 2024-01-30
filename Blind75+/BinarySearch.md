## Binary Search 

The choice between for l < r and for l <= r in binary search depends on the problem you're trying to solve.

for l < r: This is used when you're trying to find a specific element in the array, and you want to narrow down the search space to a single element. In this case, the loop continues until l and r point to the same element. This is what's happening in the findMin function. The loop continues until l and r point to the minimum element.

for l <= r: This is used when you're trying to find the position of a target value in the array. In this case, the loop continues until l and r cross each other. If the target is found, the function returns the position. If the target is not found, l will be the position where the target should be inserted to maintain the sorted order of the array. This is what's happening in the search function.

So, the choice between for l < r and for l <= r depends on whether you're trying to narrow down the search space to a single element or find the position of a target value.

When `for l < r` use `r = mid`
When `for l <= r` use `r = mid - 1`

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