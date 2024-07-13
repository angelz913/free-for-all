# 55. Jump Game

## Description

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

## Approach 1 (DP)

### Idea
`dp[i]` means whether we can reach position `i`.

### C++ Solution
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        vector<bool> dp(nums.size(), false);
        dp[0] = true;
        for (int i = 1; i < nums.size(); ++i) {
            for (int j = i - 1; j >= 0; --j) {
                if (dp[j] && (j + nums[j] >= i)) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[nums.size() - 1];
    }
};
```

### Note

Not efficient

## Approach 2 (Greedy)

### Idea

Start from the last index and keep updating the `goal`. In the end, `goal == 0` indicates that we find a path.


### C++ Solution
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int goal = nums.size() - 1;
        for (int i = goal; i >= 0; --i) {
            if (i + nums[i] >= goal) {
                goal = i;
            }
        }
        return goal == 0;
    }
};
```

## Approach 3 (Greedy)

### Idea
[Link to the original solution on LeetCode](https://leetcode.com/problems/jump-game/solutions/4534808/super-simple-intuitive-8-line-python-solution-beats-99-92-of-users/). Imagine that we are driving a car that has some amount of gas. We co


### C++ Solution
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int gas = 0;
        for (int i = 0; i < n; ++i) {
            if (gas < 0) {
                return false;
            }
            gas = max(gas, nums[i]);
            gas--;
        }
        return true;
    }
};
```