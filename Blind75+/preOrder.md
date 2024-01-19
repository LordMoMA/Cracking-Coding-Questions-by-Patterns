[144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func preorderTraversal(root *TreeNode) []int {
    res := []int{}
    if root == nil {
        return res
    }
    stack := []*TreeNode{root}

    for len(stack) > 0 {
        node := stack[len(stack) - 1]
        res = append(res, node.Val)
        stack = stack[:len(stack) - 1]

        if node.Right != nil {
            stack = append(stack, node.Right)
        }
        if node.Left != nil {
            stack = append(stack, node.Left)
        }
    }
    return res
}
```

[589. N-ary Tree Preorder Traversal](https://leetcode.com/problems/n-ary-tree-preorder-traversal/)

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func preorder(root *Node) []int {
    res := []int{}
    if root == nil {
        return res
    }
    stack := []*Node{root}

    for len(stack) > 0 {
        node := stack[len(stack)-1]
        res = append(res, node.Val)
        stack = stack[:len(stack)-1]

        for i := len(node.Children) - 1; i >= 0; i-- {
            stack = append(stack, node.Children[i])
        }
    }
    return res
}
```
