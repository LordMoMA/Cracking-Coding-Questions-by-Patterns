[605. Can Place Flowers](http://www.lintcode.com/en/problem/can-place-flowers/)

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