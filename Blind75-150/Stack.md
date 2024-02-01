
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