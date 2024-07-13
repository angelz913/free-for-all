# 110. Regular Expression Matching

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.​​​​
- `'*'` Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

Example 1:
```
Input: s = "aa", p = "a"
Output: false
```

Example 2:
```
Input: s = "aa", p = "a*"
Output: true
```

Example 3:
```
Input: s = "ab", p = ".*"
Output: true
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
            // s is empty string but p is not => can only match '*'
            ans = (p[pi - 1] == '*') ? memo(dp, s, p, si, pi - 2) : 0;
        } else if (si > 0 && pi == 0) {
            // p is empty string but s is not => no match possible
            ans = 0;
        } else if (s[si - 1] == p[pi - 1] || p[pi - 1] == '.') {
            // Match a single character => go back by one
            ans = memo(dp, s, p, si - 1, pi - 1);
        } else if (p[pi - 1] == '*') {
            char prev = p[pi - 2];
            if (prev == '.' || s[si - 1] == prev) {
                // Match a single character in s, continue matching with p,
                // or match zero characters
                ans = (memo(dp, s, p, si - 1, pi - 2) || memo(dp, s, p, si - 1, pi) ||
                       memo(dp, s, p, si, pi - 2));
            } else {
                // Match zero characters in p => go back by two
                ans = memo(dp, s, p, si, pi - 2);
            }
        } else {
            // All other cases without a match
            ans = 0;
        }
        // Store the computed result
        dp[si][pi] = ans;
        return ans;
    }

    bool isMatch(string s, string p) {
        // Remember the + 1 here.
        vector<vector<int>> dp(s.size() + 1, vector<int>(p.size() + 1, -1));
        dp[0][0] = 1;
        return memo(dp, s, p, s.size(), p.size());
    }
};
```