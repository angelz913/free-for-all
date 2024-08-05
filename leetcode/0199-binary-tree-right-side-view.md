# 199. Binary Tree Right Side View

Given the `root` of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

Example:
![example](images/0199-example.png)

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```

### Idea

BFS

### C++ Solution
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        if (root == nullptr) {
            return ans;
        }
        queue<pair<TreeNode *, int>> q;
        q.push({root, 0});
        TreeNode *prev = root;
        int level = 0;
        while (!q.empty()) {
            auto front = q.front();
            if (front.second > level) {
                ans.push_back(prev->val);
                level = front.second;
            }
            TreeNode *left = front.first->left;
            TreeNode *right = front.first->right;
            if (left) q.push({left, front.second + 1});
            if (right) q.push({right, front.second + 1});
            prev = front.first;
            q.pop();
        }
        ans.push_back(prev->val);
        return ans;
    }
};
```
