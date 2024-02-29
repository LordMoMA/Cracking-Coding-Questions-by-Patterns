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
        dfs(heights, pacific, i, 0) // left side
        dfs(heights, atlantic, i, n-1) // right side
    }

    for i := 0; i < n; i++ {
        dfs(heights, pacific, 0, i) // top side
        dfs(heights, atlantic, m-1, i) // bottom side
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

[130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/)

```go
func solve(board [][]byte) {
    if len(board) == 0 || len(board[0]) == 0 {
        return
    }

    m, n := len(board), len(board[0])
    for i := 0; i < m; i++ {
        dfs(board, i, 0)
        dfs(board, i, n-1)
    }
    for j := 0; j < n; j++ {
        dfs(board, 0, j)
        dfs(board, m-1, j)
    }

    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if board[i][j] == 'O' {
                board[i][j] = 'X'
            } else if board[i][j] == 'E' {
                board[i][j] = 'O'
            }
        }
    }
}

func dfs(board [][]byte, r, c int) {
    if r < 0 || c < 0 || r >= len(board) || c >= len(board[0]) || board[r][c] != 'O' {
        return
    }
    board[r][c] = 'E'
    dfs(board, r+1, c)
    dfs(board, r-1, c)
    dfs(board, r, c+1)
    dfs(board, r, c-1)
}
```

For the dfs, if board[r][c] == 'O', it will travese every cell in the board if there is another 'O' in the left or right or top or bottom and within board, if there is no 'O' near board[r][c], the dfs will stop?

If there are no 'O' cells adjacent to the current cell (i.e., to the left, right, top, or bottom of it), then all recursive calls of dfs from this cell will immediately return, and the function will stop traversing from this cell.

However, if the dfs function was called from multiple cells (as it typically is in a depth-first search), it will continue traversing from the other cells. The entire depth-first search only stops when all reachable 'O' cells have been visited.

```go
    m, n := len(board), len(board[0])
    for i := 0; i < m; i++ {
        dfs(board, i, 0)
        dfs(board, i, n-1)
    }
    for j := 0; j < n; j++ {
        dfs(board, 0, j)
        dfs(board, m-1, j)
    }
```

The first for loop is iterating over all rows. For each row, it starts a DFS from the first column (dfs(board, i, 0)) and the last column (dfs(board, i, n-1)). This effectively starts a DFS from all 'O's on the left and right borders of the board.

The second for loop is iterating over all columns. For each column, it starts a DFS from the first row (dfs(board, 0, j)) and the last row (dfs(board, m-1, j)). This effectively starts a DFS from all 'O's on the top and bottom borders of the board.

In each DFS, all 'O's reachable from the starting cell are marked as 'E'. So after these two loops, all 'O's that are on the border or connected to the border through other 'O's have been marked as 'E'.

```go
for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if board[i][j] == 'O' { // isolated O 
                board[i][j] = 'X'
            } else if board[i][j] == 'E' { // border connected E
                board[i][j] = 'O'
            }
        }
    }
```

The 'O's that get changed to 'X' are the ones that were not reachable from any 'O' on the border of the board in the previous depth-first search (DFS). In other words, these 'O's are not connected to the border through other 'O's.

The 'E's that get changed back to 'O' are the ones that were reachable from an 'O' on the border in the DFS. These are the 'O's that we temporarily marked as 'E' during the DFS to keep track of which cells we had visited.

In summary, the 'O's that are connected to the border remain 'O', and the 'O's that are not connected to the border (isolated 'O's) get changed to 'X'.

The dfs traversal will continue as long as it encounters cells with 'O' and within the board boundaries. If it encounters a cell with 'X' or 'E', or if it goes out of the board boundaries, it will stop and backtrack. This means that it will go back to the previous cell and continue the search in the next direction.

So, the traversal will go as far as it can find connected 'O's, not just the immediate up, down, left, and right cells. It will explore the entire connected component of 'O's starting from the cell at row r and column c.

In the dfs function, we are not transforming every 'X' to 'E'. Instead, we are transforming every 'O' that we can reach from the border to 'E'.

The condition board[r][c] != 'O' in the dfs function ensures that we only continue the search and make transformations if the current cell contains 'O'. If the cell contains 'X', the function will return immediately and not make any changes.

Then, in the solve function, we transform all remaining 'O's (those that couldn't be reached from the border) to 'X', and all 'E's (those that were reached from the border and marked as 'E' in the dfs function) back to 'O'.

This effectively captures and flips all 'O's that are not connected to the border.

[994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/description/)

```go
func orangesRotting(grid [][]int) int {
    if len(grid) == 0 || len(grid[0]) == 0 {
        return 0
    }

    m, n := len(grid), len(grid[0])
    queue := [][]int{}
    fresh := 0
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 2 {
                queue = append(queue, []int{i, j})
            } else if grid[i][j] == 1 {
                fresh++
            }
        }
    }

    if fresh == 0 {
        return 0
    }

    directions := [][]int{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}
    minutes := 0
    for len(queue) != 0 {
        size := len(queue)
        for i := 0; i < size; i++ {
            cell := queue[0]
            queue = queue[1:]
            for _, dir := range directions {
                x, y := cell[0]+dir[0], cell[1]+dir[1]
                if x < 0 || y < 0 || x >= m || y >= n || grid[x][y] != 1 {
                    continue
                }
                grid[x][y] = 2
                fresh--
                queue = append(queue, []int{x, y})
            }
        }
        minutes++
    }

    if fresh > 0 {
        return -1
    }
    return minutes - 1
}
```

The time complexity is O(M*N) because in the worst case, we might have to visit every cell in the grid once.

The space complexity is O(M*N) because in the worst case, the queue can grow as large as the number of cells in the grid.

The "Rotting Oranges" problem is typically solved using a Breadth-First Search (BFS) rather than a Depth-First Search (DFS). The reason for this is that BFS is better suited for problems where you need to find the shortest path or minimum number of steps to reach a certain condition, which is the case here where we want to find the minimum time to rot all oranges.

DFS, on the other hand, would traverse as far as possible along each path before backtracking. This means it could potentially go through all rotten oranges before even starting to rot the first fresh orange, which would not give the correct minimum time.

[286 Walls and Gates](https://leetcode.com/problems/walls-and-gates/description/)

![Walls and Gates](<Screenshot 2024-02-29 at 4.25.13 PM.png>)

```go
func wallsAndGates(rooms [][]int) {
    if len(rooms) == 0 || len(rooms[0]) == 0 {
        return
    }

    m, n := len(rooms), len(rooms[0])
    queue := [][]int{}
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if rooms[i][j] == 0 {
                queue = append(queue, []int{i, j})
            }
        }
    }

    directions := [][]int{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}
    for len(queue) != 0 {
        cell := queue[0]
        queue = queue[1:]
        for _, dir := range directions {
            x, y := cell[0]+dir[0], cell[1]+dir[1]
            if x < 0 || y < 0 || x >= m || y >= n || rooms[x][y] != 2147483647 {
                continue
            }
            rooms[x][y] = rooms[cell[0]][cell[1]] + 1
            queue = append(queue, []int{x, y})
        }
    }
}
```

### About size := len(queue) and for i := 0; i < size; i++ pattern

The size := len(queue) pattern is used to iterate over all elements in the queue at the current level of the BFS traversal. This is a common pattern in BFS, where you want to process all elements at the current level before moving on to the next level.

This pattern ensures that we process all nodes (oranges) at the current level before we increment the time and move on to the next level.

In the "Walls and Gates" problem, we don't need to track the level. We just need to fill each room with the distance to its nearest gate. The distance is calculated and updated directly in the grid as rooms[x][y] = rooms[cell[0]][cell[1]] + 1. So, we don't need to use the size := len(queue) and for i := 0; i < size; i++ pattern. We just keep processing nodes in the queue until the queue is empty.

So, whether we need to use the size := len(queue) and for i := 0; i < size; i++ pattern in BFS depends on whether we need to track the level of BFS or not.

