[226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }

    root.Left, root.Right = root.Right, root.Left

    invertTree(root.Left)
    invertTree(root.Right)
    return root
}
```

```go
func invertTree2(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    // Invert the left and right subtrees
    left := invertTree(root.Left)
    right := invertTree(root.Right)
    // Swap the left and right subtrees
    root.Left = right
    root.Right = left
    return root
}
```

