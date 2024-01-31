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

[230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

Time Complexity: O(N), where N is the number of nodes in the tree. This is because the algorithm needs to visit every node in the tree in the worst case.

Space Complexity: O(N), where N is the number of nodes in the tree. This is because the algorithm stores all the nodes of the tree in an array.

```go
func kthSmallest(root *TreeNode, k int) int {
    arr := []int{}
    inOrder(root, &arr)
    return arr[k - 1]
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

Using a stack:

Time Complexity: O(H + k), where H is the height of the tree. This is because the algorithm needs to descend to the leftmost leaf which takes H steps, then it does k steps to find the kth smallest element.

Space Complexity: O(H), where H is the height of the tree. This is because in the worst case, we have to store all the ancestors of a node in the stack, and the number of ancestors can be at most the height of the tree.

```go
func kthSmallest(root *TreeNode, k int) int {
    stack := []*TreeNode{}
    node := root

    for {
        for node != nil {
            stack = append(stack, node)
            node = node.Left
        }

        node = stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        k--
        if k == 0 {
            return node.Val
        }
        node = node.Right
    }
}
```

If space is a concern and you're dealing with a large tree, the stack method would be better. If simplicity and readability of the code are more important, the in-order traversal with an array would be better.