# 48. Rotate Image

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image **in-place**, which means you have to modify the input 2D matrix directly.

Example 1:

![example](images/0048-example-1.png)

Example 2:

![example](images/0048-example-2.png)

### C++ Solution

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        if (n == 1) {
            return;
        }
        // (s, s) represents the coordinate of the top-left corner
        // of the submatrix we are rotating. `len` is the side length.
        int s = 0;
        int len = n;
        while (s < n / 2) {
            // `i` is the offset from `s`.
            for (int i = 0; i < len - 1; ++i) {
                // (top) left to right
                int a = matrix[s][s + i];
                // (right) top to bottom
                int b = matrix[s + i][n - 1 - s];
                // (bottom) right to left
                int c = matrix[n - 1 - s][n - 1 - s - i];
                // (left) bottom to right
                int d = matrix[n - 1 - s - i][s];
                matrix[s][s + i] = d;
                matrix[s + i][n - 1 - s] = a;
                matrix[n - 1 - s][n - 1 - s - i] = b;
                matrix[n - 1 - s - i][s] = c;
            }
            s++;
            len -= 2;
        }
    }
};
```

