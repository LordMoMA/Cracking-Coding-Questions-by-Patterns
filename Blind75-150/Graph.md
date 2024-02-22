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

[695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/description/)

```go
func maxAreaOfIsland(grid [][]int) int {
    maxArea := 0
    for i := 0; i < len(grid); i++ {
        for j := 0; j < len(grid[0]); j++ {
            if grid[i][j] == 1 {
                maxArea = max(maxArea, dfs(grid, i, j))
            }
        }
    }
    return maxArea
}

func dfs(grid [][]int, r, c int) int {
    if r < 0 || c < 0 || r >= len(grid) || c >= len(grid[0]) || grid[r][c] == 0 {
        return 0
    }
    grid[r][c] = 0
    return 1 + dfs(grid, r+1, c) + dfs(grid, r, c+1) + dfs(grid, r-1, c) + dfs(grid, r, c-1)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

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

// bfs 
func cloneGraph(node *Node) *Node {
    if node == nil {
        return nil
    }

    visited := make(map[*Node]*Node)
    queue := []*Node{node}
    visited[node] = &Node{Val: node.Val, Neighbors: []*Node{}}

    for len(queue) > 0 {
        n := queue[0]
        queue = queue[1:]

        for _, neighbor := range n.Neighbors {
            if _, ok := visited[neighbor]; !ok {
                visited[neighbor] = &Node{Val: neighbor.Val, Neighbors: []*Node{}}
                queue = append(queue, neighbor)
            }
            visited[n].Neighbors = append(visited[n].Neighbors, visited[neighbor])
        }
    }

    return visited[node]
}
```

Time Complexity: O(N)

N is the number of nodes in the graph.
This is because each node in the graph is visited and cloned exactly once.
Space Complexity: O(N)

N is the number of nodes in the graph.
The space complexity is determined by the storage required for the visited map and the recursive call stack (for DFS) or the queue (for BFS).
The visited map stores a clone of each node in the graph, hence O(N).
In the worst case, the maximum depth of the recursive call stack (for DFS) or the size of the queue (for BFS) can be O(N), such as in the case of a path graph (a graph that is a single line).
So, both the DFS and BFS solutions have a time complexity of O(N) and a space complexity of O(N).


[417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

```go
func pacificAtlantic(heights [][]int) [][]int {
    if len(heights) == 0 || len(heights[0]) == 0 {
        return [][]int{}
    }

    m, n := len(heights), len(heights[0])
    pacific, atlantic := make([][]bool, m), make([][]bool, m)
    for i := 0; i < m; i++ {
        pacific[i], atlantic[i] = make([]bool, n), make([]bool, n)
    }

    for i := 0; i < m; i++ {
        dfs(heights, pacific, i, 0)
        dfs(heights, atlantic, i, n-1)
    }

    for i := 0; i < n; i++ {
        dfs(heights, pacific, 0, i)
        dfs(heights, atlantic, m-1, i)
    }

    res := [][]int{}
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if pacific[i][j] && atlantic[i][j] {
                res = append(res, []int{i, j})
            }
        }
    }

    return res
}

func dfs(heights [][]int, visited [][]bool, r, c int) {
    directions := [][]int{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}
    visited[r][c] = true

    for _, d := range directions {
        nr, nc := r+d[0], c+d[1]
        if nr < 0 || nr >= len(heights) || nc < 0 || nc >= len(heights[0]) || visited[nr][nc] || heights[nr][nc] < heights[r][c] {
            continue
        }
        dfs(heights, visited, nr, nc)
    }
}
```
The dfs function only marks a cell as true in the pacific or atlantic 2D slice if it can be reached from the starting cell of the DFS without moving "uphill" (i.e., to a cell with a higher value). This is controlled by the condition in the if statement.

```go
if nr < 0 || nc < 0 || nr >= len(heights) || nc >= len(heights[0]) || visited[nr][nc] || heights[nr][nc] < heights[r][c] {
    continue
}
```

Time Complexity: O(M*N) 
Space Complexity: O(M*N)


