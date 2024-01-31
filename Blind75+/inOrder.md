[98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

```go
func isValidBST(root *TreeNode) bool {
    arr := []int{}
    inOrder(root, &arr)
    for i := 1; i < len(arr); i++ {
        if arr[i - 1] >= arr[i] {
            return false
        }
    }
    return true

}

func inOrder(node *TreeNode, arr *[]int) {
    if node == nil {
        return
    }
    inOrder(node.Left, arr)
    *arr = append(*arr, node.Val)
    inOrder(node.Right, arr)
}
```