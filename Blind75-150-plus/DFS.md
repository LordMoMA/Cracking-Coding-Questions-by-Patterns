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

[110. Balanced Binary Tree](http://leetcode.com/problems/balanced-binary-tree/)

A binary tree is height-balanced if for every node in the tree, the difference between the height of the left subtree and the right subtree is at most 1.

The purpose of returning -1 in the DFS function when the tree is not balanced is to signal an early termination of the function.

In the context of checking if a binary tree is height-balanced, the DFS function is designed to return the height of the tree if it is balanced. However, if at any point it detects that the tree is not balanced (i.e., the difference in heights between the left and right subtrees of any node is more than 1), it returns -1.

Returning -1 serves two purposes:

It provides a way to indicate that the tree is not balanced. Since the height of a tree can never be negative, -1 is used as a special value that can't be confused with a valid height.

It allows for early termination of the function. As soon as the function detects that the tree is not balanced, it can immediately stop further computation and return -1. This can significantly improve the efficiency of the function for large trees that are not balanced.

```go
func isBalanced(root *TreeNode) bool {
    return dfs(root) != -1
}

func dfs(root *TreeNode) int {
    if root == nil {
        return 0
    }

    left := dfs(root.Left)
    if left == -1 {
        return -1
    }

    right := dfs(root.Right)
    if right == -1 {
        return -1
    }

    if abs(left - right) > 1 {
        return -1
    }

    return max(left, right) + 1
}

func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

Consider a binary tree like this:

```bash
    A
   / \
  B   C
 / \
D   E
```

In this tree, node A is the root, B and C are its children, and D and E are children of B. C, D, and E are leaf nodes.

The dfs function is called on each node to calculate its height and check if the tree is balanced at that node. The height of a node is the maximum of the heights of its left and right children, plus 1.

When dfs is called on a leaf node (C, D, or E), it returns 0 because a leaf node has no children.

When dfs is called on B, it calculates the heights of its left and right children (D and E), which are both 0. So, the height of B is max(0, 0) + 1 = 1. Since the difference in heights between D and E is 0, which is not more than 1, B is balanced.

When dfs is called on A, it calculates the heights of its left and right children (B and C). The height of B is 1 and the height of C is 0. So, the height of A is max(1, 0) + 1 = 2. However, the difference in heights between B and C is 1 - 0 = 1, which is not more than 1, so A is also balanced.

Now, let's add another node F as a child of E and call dfs on A again:

```bash
    A
   / \
  B   C
 / \
D   E
   /
  F 
```

When dfs is called on E, it calculates the height of its left child F, which is 0. So, the height of E is max(0, 0) + 1 = 1. E is balanced because it has only one child.

When dfs is called on B, it calculates the heights of its left and right children (D and E), which are 0 and 1, respectively. So, the height of B is max(0, 1) + 1 = 2. However, the difference in heights between D and E is 1 - 0 = 1, which is not more than 1, so B is also balanced.

When dfs is called on A, it calculates the heights of its left and right children (B and C). The height of B is 2 and the height of C is 0. So, the height of A is max(2, 0) + 1 = 3. However, the difference in heights between B and C is 2 - 0 = 2, which is more than 1. So, A is not balanced.

At this point, dfs returns -1, which is propagated up the call stack. So, left in the call left := dfs(root.Left) in the context of A will be -1. That's why we need to check if left == -1 { return -1 }. If left is -1, it means the left subtree is not balanced, and by extension, the entire tree is not balanced. The same logic applies to right and the right subtree.

[1448. Count Good Nodes in Binary Tree](http://leetcode.com/problems/count-good-nodes-in-binary-tree/)

The expression -int(1e4) in Go is a way to represent a large negative integer. The 1e4 is a scientific notation for 10,000, so -int(1e4) is -10,000.

The math.MinInt32 constant in Go represents the smallest possible value that a 32-bit integer can have, which is -2147483648.

```go
func goodNodes(root *TreeNode) int {
    return dfs(root, -int(1e4))
}

func dfs(node *TreeNode, maxSoFar int) int {
    if node == nil {
        return 0
    }
    count := 0
    if node.Val >= maxSoFar {
        count++
        maxSoFar = node.Val
    }
    count += dfs(node.Left, maxSoFar)
    count += dfs(node.Right, maxSoFar)
    return count
}
```

The reason we don't need to check if node.Left != nil and if node.Right != nil before calling dfs(node.Left, maxSoFar) and dfs(node.Right, maxSoFar) respectively is because the first thing we do in the dfs function is check if node == nil.

If node.Left or node.Right is nil, the dfs function will be called with nil as the node argument. Then, the if node == nil check at the start of the dfs function will be true, and the function will immediately return 0 without doing anything else. This effectively skips over nil children, so there's no need to check for nil children before calling dfs.

This is a common pattern in recursive functions that operate on binary trees. It simplifies the code by eliminating the need for nil checks before each recursive call.

Another Way of DFS:

```go
func goodNodes(root *TreeNode) int {
    count := 0
    dfs(root, math.MinInt32, &count)
    return count
}

func dfs(node *TreeNode, maxSoFar int, count *int) {
    if node == nil {
        return
    }
    if node.Val >= maxSoFar {
        *count++
        maxSoFar = node.Val
    }
    dfs(node.Left, maxSoFar, count)
    dfs(node.Right, maxSoFar, count)
}
```

[841. Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/description/)

```go
func canVisitAllRooms(rooms [][]int) bool {
    visited := make([]bool, len(rooms))
    var dfs func(room int)
    dfs = func(room int) {
        visited[room] = true
        for _, key := range rooms[room] {
            if !visited[key] {
                dfs(key)
            }
        }
    }
    dfs(0) // Start DFS from room 0
    for _, v := range visited {
        if !v {
            return false // If any room is not visited, return false
        }
    }
    return true // All rooms visited
}
```

```go
// Separate the DFS logic into its own function for clarity and reusability.

func canVisitAllRooms(rooms [][]int) bool {
    visited := make([]bool, len(rooms))
    dfs(rooms, 0, visited) // Start DFS from room 0, passing the visited slice

    // Check if all rooms have been visited
    for _, v := range visited {
        if !v {
            return false // If any room is not visited, return false
        }
    }
    return true // All rooms visited
}

// dfs function to perform depth-first search
func dfs(rooms [][]int, currentRoom int, visited []bool) {
    visited[currentRoom] = true
    for _, key := range rooms[currentRoom] {
        if !visited[key] {
            dfs(rooms, key, visited) // Visit the room that the key opens
        }
    }
}
```
