# 309. Best Time to Buy and Sell Stock with Cooldown

You are given an array `prices` where `prices[i]` is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note**: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

### Idea

We keep track of:
- `dp0`, where `dp0[i]` is the maximum profit for **not holding** a share on the `i`th day.
- `dp1`, where `dp1[i]` is the maximum profit for **holding** a share on the `i`th day.

### C++ Solution

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 1) {
            return 0;
        }
        vector<int> dp0(n, 0);        // not holding
        vector<int> dp1(n, 0);        // holding
        dp1[0] = -prices[0];
        dp1[1] = max(-prices[0], -prices[1]);
        dp0[1] = max(prices[1] - prices[0], 0);
        for (int i = 2; i < n; ++i) {
            dp0[i] = max(dp1[i - 1] + prices[i], dp0[i - 1]);
            dp1[i] = max(dp0[i - 2] - prices[i], dp1[i - 1]);
        }
        return dp0[n - 1];
    }
};
```