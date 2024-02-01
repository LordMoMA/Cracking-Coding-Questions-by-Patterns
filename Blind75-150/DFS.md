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

[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	if root == nil {
        return nil
    }

    if root.Val == p.Val || root.Val == q.Val {
        return root
    }

    left := lowestCommonAncestor(root.Left, p, q)
    right := lowestCommonAncestor(root.Right, p, q)

    if left != nil && right != nil {
        return root
    } else if left != nil {
        return left
    } else {
        return right
    }
}
```

[124. Binary Tree Maximum Path Sum](http://leetcode.com/problems/binary-tree-maximum-path-sum/)


A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once.

Consider a node in the tree. If we were to include both its left and right children in the path, then this path would go down the left child, up to the node, and then down the right child. This is not a valid path because a path in a tree must be a sequence of nodes where each pair of consecutive nodes in the sequence is connected by an edge, and it does not go upwards and then downwards.

```go
func maxPathSum(root *TreeNode) int {
    maxSum := math.MinInt32
    dfs(root, &maxSum)
    return maxSum
}

func dfs(root *TreeNode, maxSum *int) int {
    if root == nil {
        return 0
    }

    left := max(0, dfs(root.Left, maxSum))
    right := max(0, dfs(root.Right, maxSum))

    *maxSum = max(*maxSum, left + right + root.Val)

    return max(left, right) + root.Val
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

[543. Diameter of Binary Tree](http://leetcode.com/problems/diameter-of-binary-tree/)

```go
func diameterOfBinaryTree(root *TreeNode) int {
    maxDiameter := 0
    dfs(root, &maxDiameter)
    return maxDiameter
}

func dfs(root *TreeNode, maxDiameter *int) int {
    if root == nil {
        return 0
    }

    left := dfs(root.Left, maxDiameter)
    right := dfs(root.Right, maxDiameter)

    *maxDiameter = max(*maxDiameter, left + right)

    return max(left, right) + 1
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

