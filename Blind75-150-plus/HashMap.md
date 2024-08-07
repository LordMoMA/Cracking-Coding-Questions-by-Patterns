[217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/description/)

```go
func containsDuplicate(nums []int) bool {
    cache := map[int]bool{}
    for _, num := range nums {
        if cache[num] {
            return true
        }
        cache[num] = true
    }
    return false
}
```

[242. Valid Anagram](http://leetcode.com/problems/valid-anagram/description/)

```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    count := make(map[rune]int)
    for _, char := range s {
        count[char]++
    }

    for _, char := range t {
        count[char]--
        if count[char] < 0 { // the most important check
            return false
        }
    }

    return true
}
```

[1. Two Sum](https://leetcode.com/problems/two-sum/description/)

```go
func twoSum(nums []int, target int) []int {
    cache := make(map[int]int)

    for i := 0; i < len(nums); i++ {
        another := target - nums[i]
        if _, ok := cache[another]; ok {
            return []int{cache[another], i}
        }
        cache[nums[i]] = i
    }
    return nil
}
```

[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/description/)

```go
func groupAnagrams(strs []string) [][]string {
    //cache := make(map[string][]string)
    cache := map[string][]string{}
    res := [][]string{}
    for _, str := range strs {
        sortedStr := sortString(str)
        cache[sortedStr] = append(cache[sortedStr], str)
    }

    for _, group := range cache {
        res = append(res, group)
    }

    return res
}

func sortString(str string) string {
    split := strings.Split(str, "")
    sort.Strings(split)
    return strings.Join(split, "")
}
```

[128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/description/)

```go
func longestConsecutive(nums []int) int {
    numSet := make(map[int]bool)
    for _, num := range nums {
        numSet[num] = true
    }

    longestStreak := 0

    for num := range numSet {
        if !numSet[num-1] {
            currentNum := num
            currentStreak := 1

            for numSet[currentNum+1] {
                currentNum++
                currentStreak++
            }

            if currentStreak > longestStreak {
                longestStreak = currentStreak
            }
        }
    }

    return longestStreak
}
```

[535. Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/description/)
```go
import (
    "strconv"
)

type Codec struct {
    urls map[string]string
}

func Constructor() Codec {
    return Codec {
        urls: make(map[string]string),
    }
}

// Encodes a URL to a shortened URL.
func (this *Codec) encode(longUrl string) string {
	key := strconv.Itoa(len(longUrl))
    this.urls[key] = longUrl
    return "http://tinyurl.com/"+ key
}

// Decodes a shortened URL to its original URL.
func (this *Codec) decode(shortUrl string) string {
    key := shortUrl[len("http://tinyurl.com/"):]
    return this.urls[key]
}

```
[554. Brick Wall](https://leetcode.com/problems/brick-wall/description/)

```go
func leastBricks(wall [][]int) int {
    gaps := make(map[int]int)
    maxGap := 0
    for _, row := range wall {
        width := 0
        for _, brick := range row[:len(row)-1]{
            width += brick
            gaps[width]++
            if gaps[width] > maxGap {
                maxGap = gaps[width]
            }
        }
    }
    return len(wall) - maxGap
}
```
[560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/description/)

A subarray is a contiguous non-empty sequence of elements within an array.

```go
func subarraySum(nums []int, k int) int {
    sum, count := 0, 0
    sums := make(map[int]int)
    sums[0] = 1
    for _, num := range nums {
        sum += num
        if val, ok := sums[sum - k]; ok {
            count += val
        }
        sums[sum]++
    }
    return count
}
```
If the subarray doesn't need to be contiguous, the problem becomes a different one, often referred to as a "subsequence" problem.

In this case, you're looking for a subsequence (not necessarily contiguous) whose sum equals k. This problem can be solved using a depth-first search (DFS) or dynamic programming.

### Here's a DFS approach in Go:

```go
func subsequenceSum(nums []int, k int) int {
    return dfs(nums, k, 0, 0)
}

func dfs(nums []int, k, sum, idx int) int {
    if idx == len(nums) {
        if sum == k {
            return 1
        }
        return 0
    }
    return dfs(nums, k, sum+nums[idx], idx+1) + dfs(nums, k, sum, idx+1)
}
```
The line return dfs(nums, k, sum+nums[idx], idx+1) + dfs(nums, k, sum, idx+1) is exploring two possibilities at each step of the recursion:

dfs(nums, k, sum+nums[idx], idx+1): This is the case where we include the current number (nums[idx]) in our subsequence. We add the current number to our running sum (sum+nums[idx]) and move to the next index (idx+1).

dfs(nums, k, sum, idx+1): This is the case where we exclude the current number from our subsequence. We keep the running sum as it is (sum) and move to the next index (idx+1).

Backtracking is implicit in the recursion. When a recursive call is made, the current state (including the current sum and index) is pushed onto the call stack. When the recursive call returns, the state is popped from the stack, effectively undoing the changes made in the recursive call. This is the essence of backtracking.

In the line return dfs(nums, k, sum+nums[idx], idx+1) + dfs(nums, k, sum, idx+1), the first call to dfs explores the possibility of including the current number in the sum. After this call returns and its result is added to the total, the second call to dfs is made, which explores the possibility of excluding the current number from the sum. The state from the first call is not carried over to the second call, because each call operates on its own copy of the state. This is how backtracking is achieved in this code.

let's consider a simple example with an array of three elements: [1, 2, 3] and we are looking for a subsequence sum of k.

The recursive calls can be visualized as a binary tree:

```bash
                     (0, 0)  // sum = 0, idx = 0
                    /       \
          (1, 1)             (0, 1)  // include nums[0] or not
         /     \             /     \
 (3, 2)         (1, 2) (2, 2)       (0, 2)  // include nums[1] or not
 /   \          /   \   /   \       /   \
(6,3) (3,3)   (4,3) (1,3) (5,3) (2,3) (3,3) (0,3) // include nums[2] or not
```

Each node in the tree is a pair (sum, idx), where sum is the current sum and idx is the current index. The left child of each node represents the case where the number at index idx is included in the sum, and the right child represents the case where the number is not included.

The leaf nodes represent all possible subsequences of the array. For example, the leftmost leaf node (6,3) represents the subsequence [1, 2, 3], and the rightmost leaf node (0,3) represents the empty subsequence [].

The dfs function explores this tree depth-first, and for each leaf node, it checks if the sum is equal to k. The total count of subsequences with sum k is the sum of the results of all paths in the tree.

The time complexity of this solution is O(2^n), where n is the length of the input array, because in the worst case, it explores all possible subsequences of the array. The space complexity is O(n), because in the worst case, the depth of the recursion tree can go up to n.

### DP version

The dynamic programming (DP) version of the "subsequence sum equals k" problem can be solved using a 2D DP table. The idea is to build a table dp[i][j] where dp[i][j] is true if there is a subsequence of nums[0..i] with sum equal to j, and false otherwise.

```go
func subsequenceSum(nums []int, k int) bool {
    n := len(nums)
    dp := make([][]bool, n+1)
    for i := range dp {
        dp[i] = make([]bool, k+1)
    }
    dp[0][0] = true

    for i := 1; i <= n; i++ {
        for j := 0; j <= k; j++ {
            if j < nums[i-1] {
                dp[i][j] = dp[i-1][j]
            } else {
                dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i-1]]
            }
        }
    }
    return dp[n][k]
}
```
The time complexity of this dynamic programming solution is O(nk), where n is the length of the input array and k is the target sum. This is because we need to fill in a DP table with nk cells.

The space complexity is also O(n*k), because that's the size of the DP table. Each cell in the table stores a boolean value, indicating whether a subsequence with a certain sum can be formed from the first few elements of the array.


[359. Logger Rate Limiter](https://leetcode.com/problemset/?search=rate+limiter&page=1)
Design a logger system that receives a stream of messages along with their timestamps. Each unique message should only be printed at most every 10 seconds (i.e., a message printed at timestamp t will prevent other identical messages from being printed until timestamp t + 10).

All messages will come in chronological order. Several messages may arrive at the same timestamp.

Implement the Logger class:

Logger() Initializes the logger object.
bool shouldPrintMessage(int timestamp, string message) Returns true if the message should be printed in the given timestamp, otherwise returns false.

```go
package main

import "fmt"

type Logger struct {
	messages map[string]int
}

/** Initialize your data structure here. */
func Constructor() Logger {
	return Logger{messages: make(map[string]int)}
}

/*
  - Returns true if the message should be printed in the given timestamp, otherwise returns false.
    If this method returns false, the message will not be printed.
    The timestamp is in seconds granularity.
*/
func (this *Logger) ShouldPrintMessage(timestamp int, message string) bool {
	if time, ok := this.messages[message]; !ok || timestamp-time >= 10 {
		this.messages[message] = timestamp
		return true
	}
	return false
}

func main() {
	logger := Constructor()

	fmt.Println(logger.ShouldPrintMessage(1, "foo"))  // returns true
	fmt.Println(logger.ShouldPrintMessage(2, "bar"))  // returns true
	fmt.Println(logger.ShouldPrintMessage(3, "foo"))  // returns false
	fmt.Println(logger.ShouldPrintMessage(8, "bar"))  // returns false
	fmt.Println(logger.ShouldPrintMessage(10, "foo")) // returns false
	fmt.Println(logger.ShouldPrintMessage(11, "foo")) // returns true
}
```
Using closure in NodeJS 

```js
function createLogger() {
    let messages = {};

    return function(timestamp, message) {
        if (!messages[message] || timestamp - messages[message] >= 10) {
            messages[message] = timestamp;
            return true;
        }
        return false;
    };
}
```
Using class in NodeJS

```js
class Logger {
    constructor() {
        this.messages = {};
    }

    shouldPrintMessage(timestamp, message) {
        if (this.messages[message] === undefined || timestamp - this.messages[message] >= 10) {
            this.messages[message] = timestamp;
            return true;
        }
        return false;
    }
}
```

[1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/description/)

```go
func numIdenticalPairs(nums []int) int {
    count := 0
    freq := make(map[int]int)

    for _, num := range nums {
        if _, ok := freq[num]; ok {
            count += freq[num]
        }
        freq[num]++
    }

    return count
}
```
For each number, it increments the count by the current frequency of the number and then increments the frequency.

let's take an example array: nums = [1, 2, 3, 1, 1, 3].

For 1, freq[1] is not set, so we set freq[1] to 1.
For 2, freq[2] is not set, so we set freq[2] to 1.
For 3, freq[3] is not set, so we set freq[3] to 1.
For the second 1, freq[1] is 1, so we increment count by 1 and increment freq[1] to 2.
For the third 1, freq[1] is 2, so we increment count by 2 (because we can form 2 new pairs with the third 1 and the two previous 1s) and increment freq[1] to 3.
For the second 3, freq[3] is 1, so we increment count by 1 and increment freq[3] to 2.

At the end, count is 4, which is the number of good pairs in the array.

Another solution:

```go
func numIdenticalPairs(nums []int) int {
    gp := make(map[int][]int)
    sum := 0
    for i, v := range nums {
        gp[v] = append(gp[v], i)
    }
    for _, arr := range gp {
        n := len(arr)
        if n > 1 {
            sum += ((n * (n-1)) / 2)
        }
    }
    return sum
}
```

The function you provided is less efficient because it uses more memory and does more work than necessary.

Memory Usage: It creates a slice for each unique number in the array and stores the indices of that number in the slice. This uses more memory than the solution I provided, which only stores the count of each number.

Work Done: It calculates the number of pairs by computing the combination formula n*(n-1)/2 for each unique number. This is more work than necessary because you can increment the count of pairs as you go along, as in the solution I provided.

[1436. Destination City](https://leetcode.com/problems/destination-city/description/)

```go
func destCity(paths [][]string) string {
    startCities := make(map[string]bool)
    for _, path := range paths {
        startCities[path[0]] = true
    }
    
    for _, path := range paths {
        if _, exists := startCities[path[1]]; !exists {
            return path[1]
        }
    }
    return ""
}
```

[sorted dates]

there is empty space between the mm yyy and dd

```
example: ["01 Jan 1990", "05 May 1987"]
return: ["05 May 1987", "01 Jan 1990" ]
```

```go
package main

import (
    "fmt"
    "sort"
    "strings"
)

var monthToNum = map[string]string{
    "Jan": "01",
    "Feb": "02",
    "Mar": "03",
    "Apr": "04",
    "May": "05",
    "Jun": "06",
    "Jul": "07",
    "Aug": "08",
    "Sep": "09",
    "Oct": "10",
    "Nov": "11",
    "Dec": "12",
}

func sortDates(dates []string) []string {
    // Create a slice of indices and a slice of sortable dates.
    indices := make([]int, len(dates))
    sortableDates := make([]string, len(dates))

    // Convert the dates from "dd MMM yyyy" format to "yyyyMMdd" format.
    for i, date := range dates {
        indices[i] = i
        parts := strings.Split(date, " ")
        day := parts[0]
        month := parts[1]
        year := parts[2]
        sortableDates[i] = year + monthToNum[month] + day
    }

    // Sort the indices slice using the sortableDates slice.
    sort.Slice(indices, func(i, j int) bool {
        return sortableDates[indices[i]] < sortableDates[indices[j]]
    })

    // Create a new slice for the sorted dates.
    sortedDates := make([]string, len(dates))
    for i, index := range indices {
        sortedDates[i] = dates[index]
    }

    return sortedDates
}

func main() {
    dates := []string{"01 Jan 2022", "15 Feb 2023", "01 Feb 2022", "12 Jan 2023"}
    sortedDates := sortDates(dates)
    fmt.Println(sortedDates)
}
```

Solution 2:
```go
package main

import (
    "fmt"
    "sort"
)

var monthToNum = map[string]string{
    "Jan": "01",
    "Feb": "02",
    "Mar": "03",
    "Apr": "04",
    "May": "05",
    "Jun": "06",
    "Jul": "07",
    "Aug": "08",
    "Sep": "09",
    "Oct": "10",
    "Nov": "11",
    "Dec": "12",
}

var numToMonth = map[string]string{
    "01": "Jan",
    "02": "Feb",
    "03": "Mar",
    "04": "Apr",
    "05": "May",
    "06": "Jun",
    "07": "Jul",
    "08": "Aug",
    "09": "Sep",
    "10": "Oct",
    "11": "Nov",
    "12": "Dec",
}

func sortDates(dates []string) []string {
    // Convert the dates from "ddMMMyyyy" format to "yyyymmdd" format.
    for i, date := range dates {
        day := date[:2]
        month := date[3:6]
        year := date[7:]
        dates[i] = year + monthToNum[month] + day
    }

    // Use the sort.Strings function from the sort package to sort the dates.
    sort.Strings(dates)

    // Convert the dates back to "ddMMMyyyy" format.
    for i, date := range dates {
        year := date[:4]
        month := date[4:6]
        day := date[6:]
        dates[i] = day + " " + numToMonth[month] + " " + year
    }

    return dates
}

func main() {
    dates := []string{"01 Jan 2022", "15 Feb 2023", "01 Feb 2022", "12 Jan 2023"}
    sortedDates := sortDates(dates)
    fmt.Println(sortedDates)
}

```
[697. Degree of an Array](https://leetcode.com/problems/degree-of-an-array/description/)

```go
package main

import (
    "fmt"
    "math"
)

func findShortestSubArray(nums []int) int {
    // Maps to store the frequency, first occurrence, and last occurrence of each element
    frequency := make(map[int]int)
    firstOccurrence := make(map[int]int)
    lastOccurrence := make(map[int]int)

    // Iterate through the array to populate the maps
    for i, num := range nums {
        if _, exists := frequency[num]; !exists {
            firstOccurrence[num] = i
        }
        frequency[num]++
        lastOccurrence[num] = i
    }

    // Find the degree of the array
    maxFrequency := 0
    for _, freq := range frequency {
        if freq > maxFrequency {
            maxFrequency = freq
        }
    }

    // Find the minimum length of subarray with the same degree
    minLength := math.MaxInt32
    for num, freq := range frequency {
        if freq == maxFrequency {
            length := lastOccurrence[num] - firstOccurrence[num] + 1
            if length < minLength {
                minLength = length
            }
        }
    }

    return minLength
}

func main() {
    // Example usage
    nums := []int{1, 2, 2, 3, 1, 4, 2}
    fmt.Println(findShortestSubArray(nums)) // Output: 6
}
```
