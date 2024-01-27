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