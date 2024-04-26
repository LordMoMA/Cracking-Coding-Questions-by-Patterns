
[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    if list1 == nil {
        return list2
    }
    if list2 == nil {
        return list1
    }
    if list1.Val < list2.Val {
        list1.Next = mergeTwoLists(list1.Next, list2)
        return list1
    } else {
        list2.Next = mergeTwoLists(list1, list2.Next)
        return list2
    }
}
```
[912. Sort an Array](https://leetcode.com/problems/sort-an-array/description/)

```go
func sortArray(nums []int) []int {
	quickSort(nums, 0, len(nums)-1)
	return nums
}

func quickSort(nums []int, low, high int) {
	if low < high {
		pi := partition(nums, low, high)

		quickSort(nums, low, pi-1)
		quickSort(nums, pi+1, high)
	}
}

func partition(nums []int, low, high int) int {
	pivot := nums[high]
	i := low - 1
	for j := low; j <= high-1; j++ {
		if nums[j] < pivot {
			i++
			nums[i], nums[j] = nums[j], nums[i]
		}
	}
	nums[i+1], nums[high] = nums[high], nums[i+1]
	return i + 1
}

```

Time Complexity:

Best Case: O(n log n) - This is the case when the pivot element is always the median of the array. This results in the array being divided into two equal halves, leading to a balanced recursion tree.
Average Case: O(n log n) - Even in the average case, QuickSort performs well and has a time complexity of O(n log n).
Worst Case: O(n^2) - This occurs when the pivot element is the smallest or the largest element in the array, leading to an unbalanced partition where one partition contains n-1 elements and the other contains 0 elements.
Space Complexity:

The space complexity of QuickSort is O(log n). This is because the maximum depth of the recursion tree is log n (in the best and average case). Each recursive call to the QuickSort function requires its own stack frame on the call stack, so the space complexity is proportional to the maximum depth of the recursion. In the worst case, the depth of the recursion tree can be n, so the space complexity can be O(n). However, this worst case scenario can be avoided by using a technique called tail call optimization.

The above solution has "Time Limit Exceeeded" problem

The QuickSort algorithm's worst-case time complexity is O(n^2), which can occur when the array is already sorted or reverse sorted, and the pivot is chosen as the first or last element. This can lead to a "Time Limit Exceeded" error for large inputs.

To avoid this, you can modify the partition function to choose the pivot randomly instead of always choosing the last element. This will ensure that the algorithm doesn't consistently hit the worst-case scenario.


