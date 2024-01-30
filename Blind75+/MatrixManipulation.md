[48. Rotate Image](http://www.leetcode.com/problems/rotate-image/)

This line performs a four-way swap. It moves the element from the top row to the right column, the element from the right column to the bottom row, the element from the bottom row to the left column, and the element from the left column to the top row.

If you imagine the matrix as a set of concentric squares, each iteration of this loop moves one layer inward. The outermost layer is rotated first, then the next layer inward, and so on until we reach the center.

Since each layer consists of two rows (the top row and the bottom row), we only need to iterate through n/2 layers for a square matrix of size n.

For example, in a 4x4 matrix, when i=0, we're looking at the outermost layer (the entire matrix). When i=1, we're looking at the next layer inward (the 2x2 matrix in the center).


```go
func rotate(matrix [][]int) {
    n := len(matrix)
    for i := 0; i < n/2; i++ {
        for j := i; j < n-i-1; j++ {
            matrix[i][j], matrix[j][n-i-1], matrix[n-i-1][n-j-1], matrix[n-j-1][i] = matrix[n-j-1][i], matrix[i][j], matrix[j][n-i-1], matrix[n-i-1][n-j-1]
        }
    }
}
```

[54. Spiral Matrix](http://www.leetcode.com/problems/spiral-matrix/)

