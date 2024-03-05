[605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/description/)

```go
func canPlaceFlowers(flowerbed []int, n int) bool {
    count := 0
    for i := 0; i < len(flowerbed) && count < n; i++ {
        if flowerbed[i] == 0 {
            var next int
            if i == len(flowerbed)-1 {
                next = 0
            } else {
                next = flowerbed[i+1]
            }
            var prev int
            if i == 0 {
                prev = 0
            } else {
                prev = flowerbed[i-1]
            }
            if next == 0 && prev == 0 {
                flowerbed[i] = 1
                count++
            }
        }
    }
    return count == n
}
```

Solution 2:

```go
func canPlaceFlowers(flowerbed []int, n int) bool {
    count := 0
    flowerbed = append([]int{0}, flowerbed...)
    flowerbed = append(flowerbed, 0)
    for i := 1; i < len(flowerbed)-1; i++ {
        if flowerbed[i-1] == 0 && flowerbed[i] == 0 && flowerbed[i+1] == 0 {
            flowerbed[i] = 1
            count++
        }
    }
    return count >= n
}
```

Solution 3 :

```go
func canPlaceFlowers(flowerbed []int, n int) bool {
    for i := 0; i < len(flowerbed); i++ {
        if flowerbed[i] == 0 && (i == 0 || flowerbed[i-1] == 0) && (i == len(flowerbed)-1 || flowerbed[i+1] == 0) {
            flowerbed[i] = 1
            n--
        }
        if n <= 0 {
            return true
        }
    }
    return n <= 0
}
```

[169. Majority Element](https://leetcode.com/problems/majority-element/description/)

Boyer-Moore Voting Algorithm

After iterating over the entire array, major will be the majority element. This is because any element that isn't the majority will eventually cause count to decrease to 0, at which point a new candidate for the majority element will be chosen. The actual majority element will always be able to survive this process and be the final candidate.

```go
func majorityElement(nums []int) int {
    var major, count int
    for i := 0; i < len(nums); i++ {
        if count == 0 {
            major = nums[i]
            count++
        } else if major == nums[i] {
            count++
        } else {
            count--
        }
    }
    return major
}
```

[496. Next Greater Element I](https://leetcode.com/problems/majority-element)

```go
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    m := make(map[int]int)
    stack := make([]int, 0)
    for _, v := range nums2 {
        for len(stack) > 0 && stack[len(stack)-1] < v {
            m[stack[len(stack)-1]] = v
            stack = stack[:len(stack)-1]
        }
        stack = append(stack, v)
    }
    res := make([]int, len(nums1))
    for i, v := range nums1 {
        if val, ok := m[v]; ok {
            res[i] = val
        } else {
            res[i] = -1
        }
    }
    return res
}
```

Another recommended solution:

```go
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    res := make([]int, len(nums1))
    for i, num := range nums1 {
        idx := indexOf(num, nums2)
        if idx + 1 < len(nums2) {
            for _, v := range nums2[idx+1:] {
                if v > num {
                    res[i] = v
                    break // important: Once you find a number that is greater than num, you should break the loop.
                } else {
                    res[i] = -1
                }
            }  
        } else {
            res[i] = -1
        }
    }
    return res
}

func indexOf(num int, nums []int) int {
    for i, v := range nums {
        if num == v {
            return i
        }
    }
    return -1
}
```

