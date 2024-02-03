
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