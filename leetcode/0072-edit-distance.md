# 72. Edit Distance

Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

### C++ Solution

```cpp
class Solution {
public:
    int minDistance(string s, string t) {
        int m = s.size();
        int n = t.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 0; i <= m; ++i) {
            dp[i][0] = i;
        }
        for (int i = 0; i <= n; ++i) {
            dp[0][i] = i;
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s[i - 1] == t[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    int thisMin = INT_MAX;
                    thisMin = min(thisMin, dp[i - 1][j - 1]);
                    thisMin = min(thisMin, dp[i - 1][j]);
                    thisMin = min(thisMin, dp[i][j - 1]);
                    dp[i][j] = thisMin + 1;
                }
            }
        }
        return dp[m][n];
    }
};
```