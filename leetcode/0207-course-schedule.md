# 207. Course Schedule

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array prerequisites where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.
Return `true` if you can finish all courses. Otherwise, return `false`.

### Idea

Use DFS to detect cycle

### C++ Solution

```cpp
class Solution {
public:
    void dfs(vector<vector<int>> &g, int s, vector<int> &status, bool &ans) {
        if (status[s] == 1 || !ans) {
            // The node has been visited or we know there is a cycle.
            return;
        } else if (status[s] == 0) {
            // Encountered a back-edge => cycle detected.
            ans = false;
            return;
        }
        // Mark this node as explored.
        status[s] = 0;
        for (int i = 0; i < g[s].size(); ++i) {
            dfs(g, g[s][i], status, ans);
        }
        // Mark this node as visited.
        status[s] = 1;
    }

    bool canFinish(int n, vector<vector<int>>& edges) {
        // Convert the courses and requirements into a graph represented by
        // an adjacency list. The order of the edge does not matter in this problem.
        vector<vector<int>> g(n, vector<int>(0, 0));
        for (auto &edge : edges) {
            int src = edge[0];
            int dst = edge[1];
            g[src].push_back(dst);
        }
        vector<int> status(n, -1);
        bool ans = true;
        // Starting DFS for every node
        for (int i = 0; i < n; ++i) {
            dfs(g, i, status, ans);
        }
        return ans;
    }
};
```