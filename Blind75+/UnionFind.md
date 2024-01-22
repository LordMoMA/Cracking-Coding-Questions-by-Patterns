[Union Find in 5 minutes](https://www.youtube.com/watch?v=ayW5B2W9hfo&ab_channel=PotatoCoders)

rank is used to keep track of the depth of the trees representing the sets. Initially, all elements are their own set, so the depth of each tree is 0, which is the default value for elements in an int slice in Go. Therefore, there's no need to explicitly set the rank of each element to 0.

 size is used to keep track of the number of elements in each set. Initially, all elements are their own set, so each set has one element. Therefore, the size of each set is initialized to 1.

Both implementations use a similar strategy to optimize the union operation: they always attach the smaller set to the larger set to minimize the increase in depth/size. The NewUnionFind function does this by comparing the ranks (depths) of the sets, while the newUnionFind function does it by comparing the sizes of the sets.

So, both implementations are correct, and the best one to use depends on whether you want to optimize based on the depth of the sets (NewUnionFind) or the number of elements in the sets (newUnionFind).

Pattern 1:

```go
type UnionFind struct {
    parent []int
    rank   []int
    count  int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    rank := make([]int, n)
    for i := range parent {
        parent[i] = i
    }
    return &UnionFind{
        parent: parent,
        rank:   rank,
        count:  n,
    }
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) {
    xRoot, yRoot := uf.Find(x), uf.Find(y)
    if xRoot != yRoot {
        if uf.rank[xRoot] > uf.rank[yRoot] {
            uf.parent[yRoot] = xRoot
        } else if uf.rank[xRoot] < uf.rank[yRoot] {
            uf.parent[xRoot] = yRoot
        } else {
            uf.parent[yRoot] = xRoot
            uf.rank[xRoot]++
        }
        uf.count--
    }
}
```

Pattern 2:

```go
type UnionFind struct {
	parent []int
	size   []int
	count  int
}

func newUnionFind(numOfElements int) *UnionFind {
	// makeSet
	parent := make([]int, numOfElements)
	size := make([]int, numOfElements)
	for i := 0; i < numOfElements; i++ {
		parent[i] = i
		size[i] = 1
	}
	return &UnionFind{
		parent: parent,
		size:   size,
		count:  numOfElements,
	}
}

// Time: O(logn) | Space: O(1)
func (uf *UnionFind) find(node int) int {
	for node != uf.parent[node] {
		// path compression
		uf.parent[node] = uf.parent[uf.parent[node]]
		node = uf.parent[node]
	}
	return node
}

// Time: O(1) | Space: O(1)
func (uf *UnionFind) union(node1, node2 int) {
	root1 := uf.find(node1)
	root2 := uf.find(node2)

	// already in the same set
	if root1 == root2 {
		return
	}

	if uf.size[root1] > uf.size[root2] {
		uf.parent[root2] = root1
		uf.size[root1] += 1
	} else {
		uf.parent[root1] = root2
		uf.size[root2] += 1
	}

	uf.count -= 1
}
```

[323 Number of Connected Components In An Undirected Graph (Medium)](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Example 1:

Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

Output: 2

Example 2:

Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

Output: 1

Note:

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

In this problem, you're given a graph represented by a number of nodes and a list of edges. Your task is to determine how many connected components there are in the graph. A connected component is a set of nodes in which there is a path between any pair of nodes.

```go
type UnionFind struct {
    parent []int
    count  int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    for i := range parent {
        parent[i] = i
    }
    return &UnionFind{parent: parent, count: n}
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) {
    rootX, rootY := uf.Find(x), uf.Find(y)
    if rootX != rootY {
        uf.parent[rootX] = rootY
        uf.count--
    }
}

func countComponents(n int, edges [][]int) int {
    uf := NewUnionFind(n)
    for _, edge := range edges {
        uf.Union(edge[0], edge[1])
    }
    return uf.count
}
```

With "union by rank" optimization:

The "union by rank" or "union by size" optimization is used to keep the trees in the Union-Find data structure as balanced as possible. The idea is to minimize the tree height because the operations Find and Union have a time complexity proportional to the height of the tree.

When you merge two sets, if you always make the root of the smaller set point to the root of the larger set, you ensure that the height of the resulting tree is not greater than the maximum height of the two original trees. This is because the smaller tree is added as a subtree of the larger tree, so it doesn't increase the height.

```go
type UnionFind struct {
    parent []int
    rank   []int
    count  int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    rank := make([]int, n)
    for i := range parent {
        parent[i] = i
    }
    return &UnionFind{parent: parent, rank: rank, count: n}
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) {
    rootX, rootY := uf.Find(x), uf.Find(y)
    if rootX != rootY {
        if uf.rank[rootX] > uf.rank[rootY] {
            uf.parent[rootY] = rootX
        } else if uf.rank[rootX] < uf.rank[rootY] {
            uf.parent[rootX] = rootY
        } else {
            uf.parent[rootY] = rootX
            uf.rank[rootX]++
        }
        uf.count--
    }
}

func countComponents(n int, edges [][]int) int {
    uf := NewUnionFind(n)
    for _, edge := range edges {
        uf.Union(edge[0], edge[1])
    }
    return uf.count
}
```

[547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/description/)

```go
func findCircleNum(isConnected [][]int) int {
    n := len(isConnected)
    uf := NewUnionFind(n)
    for i := 0; i < n; i++ {
        for j := 0; j < i; j++ {
            if isConnected[i][j] == 1 {
                uf.Union(i, j)
            }
        }
    }
    return uf.count
}

type UnionFind struct {
    parent []int
    count int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    for i := range parent {
        parent[i] = i
    }
    return &UnionFind{
        parent: parent,
        count: n,
    }
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) {
    xRoot, yRoot := uf.Find(x), uf.Find(y)
    if xRoot != yRoot {
        uf.parent[xRoot] = yRoot
        uf.count--
    }
}
```

Rank Optimization:

```go
func findCircleNum(isConnected [][]int) int {
    n := len(isConnected)
    uf := NewUnionFind(n)
    for i := 0; i < n; i++ {
        for j := 0; j < i; j++ {
            if isConnected[i][j] == 1 {
                uf.Union(i, j)
            }
        }
    }
    return uf.count
}

type UnionFind struct {
    parent []int
    rank []int
    count int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    rank := make([]int, n)
    for i := range parent {
        parent[i] = i
        rank[i] = 1
    }
    return &UnionFind{
        parent: parent,
        rank: rank,
        count: n,
    }
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) {
    xRoot, yRoot := uf.Find(x), uf.Find(y)
    if xRoot != yRoot {
        if uf.rank[xRoot] > uf.rank[yRoot] {
            uf.parent[yRoot] = xRoot
        } else if uf.rank[xRoot] > uf.rank[yRoot] {
            uf.parent[xRoot] = yRoot
        } else {
            uf.parent[yRoot] = xRoot
            uf.rank[xRoot]++
        }
        uf.count--
    }
}
```

[261 Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/description/)

```go
func validTree(n int, edges [][]int) bool {
    if len(edges) != n-1 {
        return false
    }

    uf := NewUnionFind(n)
    for _, edge := range edges {
        uf.Union(edge[0], edge[1])
    }

    return uf.count == 1
}
```

