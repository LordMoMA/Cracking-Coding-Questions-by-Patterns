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
[987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/)

A harder problem of the above question, here it needs to keep track of the level of the node, and make sure the lower level comes before the higher level.

## BFS

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

type QueueItem struct {
    node *TreeNode
    index int
    level int
}

type Pair struct {
    val int
    level int
}

func verticalTraversal(root *TreeNode) [][]int {
    if root == nil {
        return [][]int{}
    }
    table := make(map[int][]Pair)
    queue := []QueueItem{{root, 0, 0}}
    min, max := 0, 0

    for len(queue) > 0 {
        item := queue[0]
        queue = queue[1:]
        node, index, level := item.node, item.index, item.level
        table[index] = append(table[index], Pair{node.Val, level})
        if node.Left != nil {
            queue = append(queue, QueueItem{node.Left, index - 1, level + 1})
            if min > index - 1 {
                min = index - 1
            }

        }
        if node.Right != nil {
            queue = append(queue, QueueItem{node.Right, index + 1, level + 1})
            if max < index + 1 {
                max = index + 1
            }
        }
    }

    result := make([][]int, max - min + 1)
    for i := min; i <= max; i++ {
        pairs := table[i]
        sort.Slice(pairs, func(j, k int) bool {
            if pairs[j].level == pairs[k].level {
                return pairs[j].val < pairs[k].val
            }
            return pairs[j].level < pairs[k].level
        })
        for _, pair := range pairs {
            result[i-min] = append(result[i-min], pair.val)
        }
    }
    return result
}
```

## BFS 2 

```go
import (
    "sort"
)

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type Node struct {
    node *TreeNode
    row  int
    col  int
}

func verticalTraversal(root *TreeNode) [][]int {
    if root == nil {
        return [][]int{}
    }

    nodes := []Node{}
    queue := []Node{{root, 0, 0}}

    for len(queue) > 0 {
        cur := queue[0]
        queue = queue[1:]
        nodes = append(nodes, cur)

        if cur.node.Left != nil {
            queue = append(queue, Node{cur.node.Left, cur.row + 1, cur.col - 1})
        }
        if cur.node.Right != nil {
            queue = append(queue, Node{cur.node.Right, cur.row + 1, cur.col + 1})
        }
    }

    sort.Slice(nodes, func(i, j int) bool {
        if nodes[i].col != nodes[j].col {
            return nodes[i].col < nodes[j].col
        }
        if nodes[i].row != nodes[j].row {
            return nodes[i].row < nodes[j].row
        }
        return nodes[i].node.Val < nodes[j].node.Val
    })

    result := [][]int{}
    lastCol := nodes[0].col
    column := []int{}

    for _, n := range nodes {
        if n.col != lastCol { // this is 2nd and onwards
            lastCol = n.col
            result = append(result, column)
            column = []int{}
        }
        column = append(column, n.node.Val) // this is the 1st step
    }

    result = append(result, column)

    return result
}
```
For the first node in the nodes slice, n.col is equal to lastCol (since lastCol was initialized to nodes[0].col). So, the if condition n.col != lastCol is false, and the code inside the if block is skipped. Then, the value of the first node is added to the column slice with column = append(column, n.node.Val).

For the second and subsequent nodes, the if condition n.col != lastCol is checked. If the column of the current node is different from lastCol, it means we have moved to a new column. In this case, lastCol is updated to the column of the current node, the current column is added to the result, and a new column is started. If the column of the current node is the same as lastCol, the if block is skipped.

## DFS solution

```go
import "sort"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type Node struct {
    col int
    row int
    val int
}

func verticalTraversal(root *TreeNode) [][]int {
    nodes := []Node{}
    dfs(root, 0, 0, &nodes)
    sort.Slice(nodes, func(i, j int) bool {
        if nodes[i].col != nodes[j].col {
            return nodes[i].col < nodes[j].col
        }
        if nodes[i].row != nodes[j].row {
            return nodes[i].row < nodes[j].row
        }
        return nodes[i].val < nodes[j].val
    })

    prevCol := nodes[0].col
    ans := [][]int{{}}
    for _, node := range nodes {
        if node.col != prevCol {
            prevCol = node.col
            ans = append(ans, []int{})
        }
        ans[len(ans)-1] = append(ans[len(ans)-1], node.val)
    }
    return ans
}

func dfs(node *TreeNode, row, col int, nodes *[]Node) {
    if node == nil {
        return
    }
    *nodes = append(*nodes, Node{col, row, node.Val})
    dfs(node.Left, row+1, col-1, nodes)
    dfs(node.Right, row+1, col+1, nodes)
}
```
