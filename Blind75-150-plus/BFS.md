[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func levelOrder(root *TreeNode) [][]int {
    res := [][]int{}
    if root == nil {
        return res
    }
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        level := []int{}
        levelSize := len(queue)
        for i := 0; i < levelSize; i++ {
            curr := queue[0]
            queue = queue[1:]
            level = append(level, curr.Val)
            if curr.Left != nil {
                queue = append(queue, curr.Left)
            }
            if curr.Right != nil {
                queue = append(queue, curr.Right)
            } 
        }
        res = append(res, level)
    }
    return res
}
```

[199. Binary Tree Right Side View](http://leetcode.com/problems/binary-tree-right-side-view/)

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func rightSideView(root *TreeNode) []int {
    res := []int{}
    if root == nil {
        return res
    }
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        levelSize := len(queue)
        for i := 0; i < levelSize; i++ {
            curr := queue[0]
            queue = queue[1:]
            if i == levelSize - 1 {
                res = append(res, curr.Val)
            }
            if curr.Left != nil {
                queue = append(queue, curr.Left)
            }
            if curr.Right != nil {
                queue = append(queue, curr.Right)
            }
        }
    }
    return res
}

func rightSideView2(root *TreeNode) []int {
    res := []int{}
    if root == nil {
        return res
    }
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        level := []int{}
        levelSize := len(queue)
        for i := 0; i < levelSize; i++ {
            curr := queue[0]
            queue = queue[1:]
            level = append(level, curr.Val)
            if curr.Left != nil {
                queue = append(queue, curr.Left)
            } 
            if curr.Right != nil {
                queue = append(queue, curr.Right)
            }
        }
        res = append(res, level[len(level)-1])
    }
    return res
}

```

DFS not intuitive:

```go
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func rightSideView(root *TreeNode) []int {
    if root == nil {
        return nil
    }

    var result []int
    dfs(root, 0, &result)
    return result
}

func dfs(node *TreeNode, level int, result *[]int) {
    if node == nil {
        return
    }

    if level == len(*result) {
        *result = append(*result, node.Val)
    }

    dfs(node.Right, level+1, result)
    dfs(node.Left, level+1, result)
}
```

[1448. Count Good Nodes in Binary Tree](http://leetcode.com/problems/count-good-nodes-in-binary-tree/)

Better to be solved by DFS

```go
type NodeMax struct {
    Node *TreeNode
    Max  int
}

func goodNodes(root *TreeNode) int {
    if root == nil {
        return 0
    }

    count := 0
    queue := []NodeMax{{Node: root, Max: root.Val}}

    for len(queue) > 0 {
        curr := queue[0]
        queue = queue[1:]

        if curr.Node.Val >= curr.Max {
            count++
        }

        if curr.Node.Left != nil {
            queue = append(queue, NodeMax{Node: curr.Node.Left, Max: max(curr.Max, curr.Node.Left.Val)})
        }

        if curr.Node.Right != nil {
            queue = append(queue, NodeMax{Node: curr.Node.Right, Max: max(curr.Max, curr.Node.Right.Val)})
        }
    }

    return count
}

```

traverse a binary tree and group the nodes by their column in the tree. This is a common problem in binary tree traversal and can be solved using a breadth-first search (BFS) algorithm with a slight modification to keep track of the column each node is in.

// Input: pointer to root node
//    
//    1
//  /  \
// 7    2
//     / \
//    3   8
         /
        5
// output [[1,3][2,5].[8]]

```go
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type QueueItem struct {
    Node  *TreeNode
    Index int
}

func verticalOrder(root *TreeNode) [][]int {
    if root == nil {
        return [][]int{}
    }

    columnTable := make(map[int][]int)
    min, max := 0, 0
    queue := []QueueItem{{root, 0}}

    for len(queue) > 0 {
        item := queue[0]
        queue = queue[1:]

        node, index := item.Node, item.Index
        columnTable[index] = append(columnTable[index], node.Val)

        if node.Left != nil {
            queue = append(queue, QueueItem{node.Left, index - 1})
            if min > index-1 {
                min = index - 1
            }
        }

        if node.Right != nil {
            queue = append(queue, QueueItem{node.Right, index + 1})
            if max < index+1 {
                max = index + 1
            }
        }
    }

    result := make([][]int, max-min+1)
    for i := min; i <= max; i++ {
        result[i-min] = columnTable[i]
    }

    return result
}
```
