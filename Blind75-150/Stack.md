
[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

```go
func isValid(s string) bool {
    stack := []rune{}

    for _, char := range s {
        switch char {
        case '(', '[', '{':
            stack = append(stack, char)
        case ')':
            if len(stack) == 0 || stack[len(stack)-1] != '(' {
                return false
            }
            stack = stack[:len(stack)-1]
        case ']':
            if len(stack) == 0 || stack[len(stack)-1] != '[' {
                return false
            }
            stack = stack[:len(stack)-1]
        case '}':
            if len(stack) == 0 || stack[len(stack)-1] != '{' {
                return false
            }
            stack = stack[:len(stack)-1]
        }
    }

    return len(stack) == 0
}
```

Use an extra map to store the parentheses pairs:

```go
func isValid(s string) bool {
    cache := map[byte]byte{
        '(': ')',
        '{': '}',
        '[': ']',
    }
    stack := []byte{}
    for i := 0; i < len(s); i++ {
        sub := s[i]
        if sub == '(' || sub == '{' || sub == '[' {
            stack = append(stack, sub)
        } else if len(stack) > 0 && cache[stack[len(stack)-1]] == sub {
            stack = stack[:len(stack)-1]
        } else {
            return false
        }
    }
    return len(stack) == 0
}

```

[155. Min Stack](http://leetcode.com/problems/min-stack/)

```go
type MinStack struct {
    stack []int
    min   []int
}

/** initialize your data structure here. */
func Constructor() MinStack {
    return MinStack{}
}

func (this *MinStack) Push(x int) {
    this.stack = append(this.stack, x)
    if len(this.min) == 0 || x <= this.min[len(this.min)-1] { // equal should be included
        this.min = append(this.min, x)
    }
}

func (this *MinStack) Pop() {
    // if statement should be checked first, otherwise it will cause index out of range, when the stack is empty
    if this.stack[len(this.stack)-1] == this.min[len(this.min)-1] {
        this.min = this.min[:len(this.min)-1]
    }
    this.stack = this.stack[:len(this.stack)-1]
}

func (this *MinStack) Top() int {
    return this.stack[len(this.stack)-1]
}

func (this *MinStack) GetMin() int {
    return this.min[len(this.min)-1]
}
```

[150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

In Go, you cannot directly convert a string to an operator and use it. However, you can use a switch statement or if-else conditions to check the string and perform the corresponding operation.

```go
func evalRPN(tokens []string) int {
    stack := []int{}
    for _, token := range tokens {
        switch token {
        case "+", "-", "*", "/":
            b := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            a := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            switch token {
            case "+":
                stack = append(stack, a+b)
            case "-":
                stack = append(stack, a-b)
            case "*":
                stack = append(stack, a*b)
            case "/":
                stack = append(stack, a/b)
            }
        default:
            num, _ := strconv.Atoi(token)
            stack = append(stack, num)
        }
    }
    return stack[0]
}
```

[739. Daily Temperatures](http://leetcode.com/problems/daily-temperatures/)

```go
func dailyTemperatures(temps []int) []int {
    n := len(temps)
    res := make([]int, n)
    stack := []int{} // maintain the indices of the "hotter" days.
    for i := 0; i < n; i++ {
        for len(stack) > 0 && temps[i] > temps[stack[len(stack)-1]] {
            lastIdx := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            res[lastIdx] = i - lastIdx
        }
        stack = append(stack, i)
    }
    return res
}
```

[853. Car Fleet](https://leetcode.com/problems/car-fleet/)

```go
func carFleet(target int, position []int, speed []int) int {
    n := len(position)
    cars := make([][2]int, n)
    for i := 0; i < n; i++ {
        cars[i] = [2]int{position[i], speed[i]}
    }
    sort.Slice(cars, func(i, j int) bool {
        return cars[i][0] > cars[j][0]
    })
    fleets := 0
    time := 0.0
    for i := 0; i < n; i++ {
        curTime := float64(target-cars[i][0]) / float64(cars[i][1])
        if curTime > time {
            time = curTime
            fleets++
        }
    }
    return fleets
}
```

We iterate over the sorted cars. If a car needs more time to reach the target than the current maximum time cur, it cannot catch up with the car fleet that the previous car belongs to, so we increment the result res and update cur.

```go
type car struct {
    position int
    time     float64
}

func carFleet(target int, position []int, speed []int) int {
    cars := make([]car, len(position))
    for i := 0; i < len(position); i++ {
        cars[i] = car{position: position[i], time: float64(target-position[i]) / float64(speed[i])}
    }
    sort.Slice(cars, func(i, j int) bool {
        return cars[i].position > cars[j].position
    })

    res, cur := 0, 0.0
    for _, c := range cars {
        if c.time > cur {
            cur = c.time
            res++
        }
    }
    return res
}
```

The expression float64(target-position[i]) / float64(speed[i]) is used instead of float64((target-position[i]) / speed[i]) to ensure that the division is done in floating-point arithmetic, not integer arithmetic.

In Go, if both operands of the / operator are integers, then integer division is performed, and the result is an integer. This means that any fractional part is discarded. For example, 5 / 2 equals 2, not 2.5.
