[200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

```go   
func numIslands(grid [][]byte) int {
    if len(grid) == 0 || len(grid[0]) == 0 {
        return 0
    }
    count := 0
    for i := 0; i < len(grid); i++ {
        for j := 0; j < len(grid[0]); j++ {
            if grid[i][j] == '1' {
                count++
                dfs(grid, i, j)
            }
        }
    }
    return count
}

func dfs(grid [][]byte, r, c int) {
    if r < 0 || c < 0 || r >= len(grid) || c >= len(grid[0]) || grid[r][c] == '0' {
        return
    }
    grid[r][c] = '0'
    dfs(grid, r+1, c)
    dfs(grid, r, c+1)
    dfs(grid, r-1, c)
    dfs(grid, r, c-1)
}

// bfs

func numIslands2(grid [][]byte) int {
    if len(grid) == 0 || len(grid[0]) == 0 {
        return 0
    }

    count := 0
    for i := 0; i < len(grid); i++ {
        for j := 0; j < len(grid[0]); j++ {
            if grid[i][j] == '1' {
                bfs(grid, i, j)
                count++
            }
        }
    }
    return count
}

func bfs(grid [][]byte, i, j int) {
    queue := [][]int{{i, j}}
    directions := [][]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

    for len(queue) != 0 {
        cell := queue[0]
        queue = queue[1:]
        for _, dir := range directions {
            x, y := cell[0]+dir[0], cell[1]+dir[1]
            if x < 0 || y < 0 || x >= len(grid) || y >= len(grid[0]) || grid[x][y] == '0' {
                continue
            }
            queue = append(queue, []int{x, y})
            grid[x][y] = '0'
        }
    }
}
```
Time Complexity:

For both DFS and BFS, the time complexity is O(M*N), where M is the number of rows and N is the number of columns in the grid. This is because in the worst-case scenario, we might have to visit every cell in the grid once.

Space Complexity:

For DFS, the space complexity is O(MN) in the worst case due to recursion. This happens when the grid map is filled with lands where DFS goes by MN deep.

For BFS, the space complexity is O(min(M, N)) because in worst case you could end up adding all the cells in the grid into the queue. However, in practice, it will be the maximum size of the queue, which depends on the size of the largest level in the graph (i.e., the maximum number of nodes reachable within the same number of steps from the start node). In the case of a grid, this would be min(M, N).

[133. Clone Graph](https://leetcode.com/problems/clone-graph/)

```go
func cloneGraph(node *Node) *Node {
    return dfs(node, make(map[*Node]*Node))
}

func dfs(node *Node, visited map[*Node]*Node) *Node {
    if node == nil {
        return nil
    }

    if _, ok := visited[node]; ok {
        return visited[node]
    }

    clone := &Node{Val: node.Val, Neighbors: make([]*Node, len(node.Neighbors))}
    visited[node] = clone

    for i, neighbor := range node.Neighbors {
        clone.Neighbors[i] = dfs(neighbor, visited)
    }

    return clone
}
```