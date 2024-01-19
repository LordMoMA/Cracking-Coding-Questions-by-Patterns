[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```go
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
        return nil
    }
    root := &TreeNode{Val: preorder[0]}
    if len(preorder) == 1 {
        return root
    }
    idx := 0
    for ; idx < len(inorder); idx++ {
        if inorder[idx] == preorder[0] {
            break
        }
    }
    root.Left = buildTree(preorder[1:idx+1], inorder[:idx])
    root.Right = buildTree(preorder[idx+1:], inorder[idx+1:])
    return root
}
```

[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```go
func buildTree(inorder []int, postorder []int) *TreeNode {
    if len(postorder) == 0 {
        return nil
    }
    root := &TreeNode{Val: postorder[len(postorder)-1]}
    if len(postorder) == 1 {
        return root
    }
    idx := 0
    for ; idx < len(inorder); idx++ {
        if inorder[idx] == postorder[len(postorder)-1] {
            break
        }
    }
    root.Left = buildTree(inorder[:idx], postorder[:idx])
    root.Right = buildTree(inorder[idx+1:], postorder[idx:len(postorder)-1])
    return root
}
```

[889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

```go
func constructFromPrePost(pre []int, post []int) *TreeNode {
    if len(pre) == 0 {
        return nil
    }
    root := &TreeNode{Val: pre[0]}
    if len(pre) == 1 {
        return root
    }
    idx := 0
    for ; idx < len(post); idx++ {
        if post[idx] == pre[1] {
            break
        }
    }
    root.Left = constructFromPrePost(pre[1:idx+2], post[:idx+1])
    root.Right = constructFromPrePost(pre[idx+2:], post[idx+1:len(post)-1])
    return root
}
```
