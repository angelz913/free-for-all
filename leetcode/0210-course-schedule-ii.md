# 210. Course Schedule II

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array prerequisites where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an **empty array**.

### Idea

Topological Sort.

### C++ Solution

```cpp
class Solution {
public:
    void dfs(vector<vector<int>> &g, int s, vector<int> &status,
             vector<int> &ans, bool &isPossible, int &pos) {
        if (status[s] == 1) {
            // The node has been visited or we know there is a cycle.
            return;
        } else if (status[s] == 0) {
            // Encountered a back-edge => cycle detected.
            isPossible = false;
            return;
        }
        status[s] = 0;
        for (int i = 0; i < g[s].size(); ++i) {
            dfs(g, g[s][i], status, ans, isPossible, pos);
        }
        status[s] = 1;
        ans[pos] = s;
        pos--;
    }

    vector<int> findOrder(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n, vector<int>(0));
        // Convert the courses and requirements into a graph represented by
        // an adjacency list. The order of the edge matters in this problem.
        for (auto &edge : edges) {
            int src = edge[1];
            int dst = edge[0];
            g[src].push_back(dst);
        }
        vector<int> status(n, -1);
        vector<int> ans(n, 0);
        bool isPossible = true;
        int pos = n - 1;
        // Use DFS for topological sort.
        for (int i = 0; i < n; ++i) {
            dfs(g, i, status, ans, isPossible, pos);
        }
        if (!isPossible) {
            vector<int> empty;
            return empty;
        } else {
            return ans;
        }
    }
};
```