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

[104. Maximum Depth of Binary Tree](http://leetcode.com/problems/maximum-depth-of-binary-tree/)

plus 1 (for the root node itself). This is done by recursively calling maxDepth on root.Left and root.Right, and then taking the maximum of the two depths and adding 1 to it for the current node.

```go
func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    return max(maxDepth(root.Left), maxDepth(root.Right)) + 1
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

[100. Same Tree](https://leetcode.com/problems/same-tree/description/)

```go
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSameTree(p *TreeNode, q *TreeNode) bool {
    if p == nil && q == nil {
        return true
    }
    if p == nil || q == nil || p.Val != q.Val {
        return false
    }

    return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```

[572. Subtree of Another Tree](http://leetcode.com/problems/subtree-of-another-tree/)

```go
func isSubtree(s *TreeNode, t *TreeNode) bool {
    if s == nil {
        return false
    }
    if isSameTree(s, t) {
        return true
    }
    return isSubtree(s.Left, t) || isSubtree(s.Right, t)
}

func isSameTree(p *TreeNode, q *TreeNode) bool {
    if p == nil && q == nil {
        return true
    }
    if p == nil || q == nil || p.Val != q.Val {
        return false
    }

    return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```



