[929. Unique Email Addresses](https://leetcode.com/problems/unique-email-addresses/description/)

```go
func numUniqueEmails(emails []string) int {
    uniqueEmails := make(map[string]bool)

    for _, email := range emails {
        parts := strings.Split(email, "@")
        local := strings.Split(parts[0], "+")[0]
        local = strings.ReplaceAll(local, ".", "")
        uniqueEmails[local+"@"+parts[1]] = true
    }

    return len(uniqueEmails)
}
```