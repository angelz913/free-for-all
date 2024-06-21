# Dynamic Programming

## 22. Generate Parentheses

**Description**

Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
 
Example 1:
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

Example 2:
```
Input: n = 1
Output: ["()"]
```

Constraints:
- `1 <= n <= 8`

**Idea**

We keep track of:
- The total number of `(` currently in `s`
- The number of `)` minus the number of of `(` in `s`

Then, if the number of `(` equals the number of `)`, we can only add `(`. And if the number of `(` reaches `n`, we can only add `)`.  

**C++ Solution**

```cpp
class Solution {
public:
    void buildSolution(vector<string> &ans, string s, int k, int l, int n) {
        if (k == 0 && l == n && s.size() == 2 * n) {
            ans.push_back(s);
        }
        else if (k > n || l > n) {
            return;
        }
        else if (k == 0) {
            buildSolution(ans, s + "(", k + 1, l + 1, n);
        }
        else if (l == n) {
            buildSolution(ans, s + ")", k - 1, l, n);
        }
        else {
            buildSolution(ans, s + "(", k + 1, l + 1, n);
            buildSolution(ans, s + ")", k - 1, l, n);
        }
        
    }

    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        string s;
        buildSolution(ans, s, 0, 0, n);
        return ans;
    }
};
```
