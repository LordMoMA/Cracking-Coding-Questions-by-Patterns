[162. Find Peak Element](https://leetcode.com/problems/find-peak-element/description/)

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

```go
// "BEEHIVE"  // Output: 6
func findDecreasingChar(s string) int {
    for i := 1; i < len(s); i++ {
        if s[i] < s[i-1] {
            return i
        }
    }
    return -1
}
```
