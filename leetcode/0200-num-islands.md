# 200. Number of Islands

Given an `m x n` 2D binary grid grid which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

Example 2:

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

### Idea

DFS

### C++ Solution
```cpp
class Solution {
public:
    void visit(vector<vector<char>> &grid, int i, int j) {
        int m = grid.size();
        int n = grid[0].size();
        // Boundary check.
        if (i < 0 || j < 0 || i >= m || j >= n ) {
            return;
        }
        // Ignore water.
        if (grid[i][j] != '1') {
            return;
        }
        // Mark as visited (by turning land into water).
        grid[i][j] = '0';
        visit(grid, i - 1, j);
        visit(grid, i + 1, j);
        visit(grid, i, j - 1);
        visit(grid, i, j + 1);
    }

    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                // Start DFS upon finding a land, and increment counter
                if (grid[i][j] == '1') {
                    ans++;
                    visit(grid, i, j);
                }
            }
        }
        return ans;
    }
};

```