# 44. Wildcard Matching

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'?'` and `'*'` where:

- `'?'` Matches any single character.​​​​
- `'*'` Matches sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

Example 1:
```
Input: s = "aa", p = "a"
Output: false
```

Example 2:
```
Input: s = "aa", p = "*"
Output: true
```

Example 3:
```
Input: s = "cb", p = "?a"
Output: false
```

### Idea
- DP with memoization
- `dp[i][j]` means whether the strings `s[0:i+1]` and `p[0:j+1]` can be matched

### C++ Solution

```cpp
class Solution {
public:
    bool memo(vector<vector<int>> &dp, const string &s, const string &p,
              int si, int pi) {
        // Already computed
        if (dp[si][pi] > -1) {
            return dp[si][pi];
        }
        bool ans = 0;
        if (si == 0 && pi > 0) {
            // Can only match when p ends with '*'
            ans = (p[pi - 1] == '*') ? memo(dp, s, p, si, pi - 1) : 0;
        } else if (si > 0 && pi == 0) {
            // No match possible
            ans = 0;
        } else if (s[si - 1] == p[pi - 1] || p[pi - 1] == '?') {
            // Match one character
            ans = memo(dp, s, p, si - 1, pi - 1);
        } else if (p[pi - 1] == '*') {
            // Match zero, one or many characters
            ans = (memo(dp, s, p, si - 1, pi - 1) || memo(dp, s, p, si - 1, pi) ||
                   memo(dp, s, p, si, pi - 1));
        } else {
            ans = 0;
        }
        // Store the answer
        dp[si][pi] = ans;
        return ans;
    }

    bool isMatch(string s, string p) {
        vector<vector<int>> dp(s.size() + 1, vector<int>(p.size() + 1, -1));
        dp[0][0] = 1;
        return memo(dp, s, p, s.size(), p.size());
    }
};
```