# 122. Best Time to Buy and Sell Stock II

### Description

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return the **maximum** profit you can achieve.

### Idea

We keep track of:
- `dp0`: the maximum profit for **not holding** a share on the previous day.
- `dp1`: the maximum profit for **holding** a share on the previous day.

### C++ Solution

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int dp0 = 0;             // not holding
        int dp1 = -prices[0];    // holding
        for (int i = 1; i < prices.size(); ++i)
        {
            dp0 = max(dp0, dp1 + prices[i]);
            dp1 = max(dp1, dp0 - prices[i]);
        }
        return dp0;
    }
};
```
