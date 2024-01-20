![Alt text](pre-in-post-order-traversal.png)

[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```go
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
        return nil
    }
    root := &TreeNode{Val: preorder[0]}
    mid := indexOf(inorder, preorder[0])
    root.Left = buildTree(preorder[1:mid+1], inorder[:mid])
    root.Right = buildTree(preorder[mid+1:], inorder[mid+1:])
    return root
}

func indexOf(arr []int, val int) int {
    for i := range arr {
        if arr[i] == val {
            return i
        }
    }
    return -1
}

func buildTree2(preorder []int, inorder []int) *TreeNode {
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
    root.Left = buildTree2(preorder[1:idx+1], inorder[:idx])
    root.Right = buildTree2(preorder[idx+1:], inorder[idx+1:])
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
    mid := indexOf(inorder, root.Val)
    root.Left = buildTree(inorder[:mid], postorder[:mid])
    root.Right = buildTree(inorder[mid+1:], postorder[mid:len(postorder)-1])
    return root
}

func indexOf(arr []int, val int) int {
    for i := range arr {
        if arr[i] == val {
            return i
        }
    }
    return -1
}

func buildTree2(inorder []int, postorder []int) *TreeNode {
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
    root.Left = buildTree2(inorder[:idx], postorder[:idx])
    root.Right = buildTree2(inorder[idx+1:], postorder[idx:len(postorder)-1])
    return root
}
```

[889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

```go

func constructFromPrePost(preorder []int, postorder []int) *TreeNode {
    if len(postorder) == 0 {
        return nil
    }
    root := &TreeNode{Val: postorder[len(postorder)-1]}
    if len(postorder) == 1 {
        return root
    }
    idx := indexOf(preorder[1:], postorder[len(postorder)-2]) // finding the index of the root of the right subtree in the preorder array.
    root.Left = constructFromPrePost(preorder[1:idx+1], postorder[:idx])
    root.Right = constructFromPrePost(preorder[idx+1:], postorder[idx:len(postorder)-1])
    return root
}

func constructFromPrePost2(pre []int, post []int) *TreeNode {
    if len(pre) == 0 {
        return nil
    }

    root := &TreeNode{Val: pre[0]}
    if len(post) == 1 {
        return root
    }
    idx := indexOf(post, pre[1])
    root.Left = constructFromPrePost2(pre[1:idx+2], post[:idx+1])
    root.Right = constructFromPrePost2(pre[idx+2:], post[idx+1:len(post)-1])
    return root
}

func indexOf(arr []int, val int) int {
    for i := range arr {
        if arr[i] == val {
            return i
        }
    }
    return -1
}

func constructFromPrePost3(pre []int, post []int) *TreeNode {
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
    root.Left = constructFromPrePost3(pre[1:idx+2], post[:idx+1]) // preorder starts its left from 1, so to include everything in the left subtree, we need to add 2 to idx.
    root.Right = constructFromPrePost3(pre[idx+2:], post[idx+1:len(post)-1])
    return root
}
```
