![Alt text](images/pre-in-post-order-traversal.png)

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

[297. Serialize and Deserialize Binary Tree](http://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

If you serialize a binary tree using "#" to represent null nodes, the serialized string would look something like this:

"1,2,#,#,3,4,#,#,5,#,#"

This represents the following binary tree:

```bash
    1
   / \
  2   3
     / \
    4   5
```

Depth-First Search (DFS) pattern, specifically Pre-Order Traversal.:

Time: O(n) n is the number of nodes in the tree
Space:
serialize: O(n), where n is the height of the tree.
deserialize: O(n), where n is the number of nodes in the tree.

```go
import (
    "strings"
    "strconv"
)

type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}

type Codec struct {
}

func Constructor() Codec {
    return Codec{}
}

// Serializes a tree to a single string.
func (this *Codec) serialize(root *TreeNode) string {
    if root == nil {
        return "#"
    }
    return strconv.Itoa(root.Val) + "," + this.serialize(root.Left) + "," + this.serialize(root.Right)
}

// Deserializes your encoded data to tree.
func (this *Codec) deserialize(data string) *TreeNode {
    list := strings.Split(data, ",")
    return this.buildTree(&list)
}

//  It uses pre-order traversal (root, left, right) to build the tree.
func (this *Codec) buildTree(list *[]string) *TreeNode {
    if (*list)[0] == "#" {
        *list = (*list)[1:]
        return nil
    }
    val, _ := strconv.Atoi((*list)[0])
    *list = (*list)[1:]
    node := &TreeNode{Val: val}
    node.Left = this.buildTree(list)
    node.Right = this.buildTree(list)
    return node
}
```

Breadth-First Search (BFS) or Level-Order Traversal:

Time: O(n)
Space:
serialize: O(n), where n is the maximum width of the tree. This is due to the maximum number of nodes stored in the queue.
deserialize: O(n), where n is the maximum width of the tree. This is due to the maximum number of nodes stored in the queue.

```go
type Codec struct {
    
}

func Constructor() Codec {
    return Codec{}
}

// Serializes a tree to a single string.
func (this *Codec) serialize(root *TreeNode) string {
    res := []string{}
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        if node == nil {
            res = append(res, "null")
        } else {
            res = append(res, strconv.Itoa(node.Val))
            queue = append(queue, node.Left)
            queue = append(queue, node.Right)
        }
    }
    return strings.Join(res, ",")
}

// Deserializes your encoded data to tree.
func (this *Codec) deserialize(data string) *TreeNode {
    if data == "" {
        return nil
    }
    arr := strings.Split(data, ",")
    root := &TreeNode{}
    root.Val, _ = strconv.Atoi(arr[0])
    queue := []*TreeNode{root}
    i := 1
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        if arr[i] != "null" {
            left, _ := strconv.Atoi(arr[i])
            node.Left = &TreeNode{Val: left}
            queue = append(queue, node.Left)
        }
        i++
        if arr[i] != "null" {
            right, _ := strconv.Atoi(arr[i])
            node.Right = &TreeNode{Val: right}
            queue = append(queue, node.Right)
        }
        i++
    }
    return root
}

// Your Codec object will be instantiated and called as such:
// var ser Codec
// var deser Codec
// deser.deserialize(ser.serialize(root))
```

The BFS approach has the advantage of being able to handle trees that are very deep without risking a stack overflow, which could be a problem for the DFS approach if the tree is not balanced. However, the BFS approach could use more space than the DFS approach if the tree is very wide, because it needs to store all the nodes at a level in the queue at the same time.

