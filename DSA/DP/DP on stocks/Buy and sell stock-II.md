# Buy and sell stock-II
- We are given an array Arr[] of length n. It represents the price of a stock on ‘n’ days. The following guidelines need to be followed:

   1. We can buy and sell the stock any number of times.
   2. In order to sell the stock, we need to first buy it on the same or any previous day.
   3. We can’t buy a stock again after buying it once. In other words, we first buy a stock and then sell it. After selling we can buy and sell again. But we can’t sell before buying and can’t buy before selling any previously bought stock.

### Logic
- Here, we need to try out all the possible ways to buy and sell, and get the best possible way for max profit. As we are referring to "try all ways- find best possible way", we go for recursion.

### Steps to write the recursion:
1. Express everything in terms of (ind, buy).
2. Explore all possibilities on that day.
3. Take the max of all the profits made
4. Base case

- f(0,1) = Starting on 0th day, with buy-1, what maximum profit can be made.


### Recursion
```
int f(int ind, int buy)
{
      // Base case
      if (ind == n) return 0;
   
      // If we are allowed to buy
      if (buy) {
         // Either buy the stock or skip it
         profit =  max(-prices[ind] + f(ind + 1, 0), f(ind + 1, 1));
      } else {
         // If we have bought the stock, we can either sell it or skip it
         profit = max(prices[ind] + f(ind + 1, 1), f(ind + 1, 0));
      }
      return profit;
}
```
- Time complexity:O(2^n) (for each day, we have two choices: buy or not buy)
- Space complexity:O(n) (for the recursion stack)

- Here, if we observe, we are calculating the same subproblems again and again. For example, if we are at day 0, and we have bought the stock, then we will calculate the profit for day 1 with buy=0, and then again for day 1 with buy=1. This is a classic case of overlapping subproblems, which can be optimized using memoization or tabulation.
- if the value of buy is 1, we are allowed to buy the stock, and if it is 0, we are allowed to sell the stock.

### Memoization
```
int f(int ind, int buy, vector<vector<int>>& dp)
{
      // Base case
      if (ind == n) return 0;
      if (dp[ind][buy] != -1) return dp[ind][buy];

      // If we are allowed to buy
      if (buy) {
         // Either buy the stock or skip it
         dp[ind][buy] = max(-prices[ind] + f(ind + 1, 0, dp), f(ind + 1, 1, dp));
      } else {
         // If we have bought the stock, we can either sell it or skip it
         dp[ind][buy] = max(prices[ind] + f(ind + 1, 1, dp), f(ind + 1, 0, dp));
      }
      return dp[ind][buy];
}
```
- Time complexity:O(n * 2) = O(n) (since we are storing the results of subproblem)
- Space complexity:O(n) (auxiliary space for recursion stack) + O(n * 2) = O(n) (for the dp array)

### Steps for tabulation:
1. write base case
2. write all the parameters.
3. copy the recurrence code. 
4. change the recursion to iteration.
5. fill the dp array in reverse order. i.e., in recursion we go from 0 to n, but in tabulation we go from n-1 to 0.

### Tabulation
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n+1,vector<int>(2,0));
        dp[n][0]=dp[n][1]=0;
        for(int i=n-1;i>=0;i--)
        {
            for(int buy=0;buy<=1;buy++)
            {
                if(buy)
                {
                    dp[i][buy] = max(-prices[i]+dp[i+1][0],dp[i+1][1]);
                }
                else
                {
                    dp[i][buy] = max(prices[i]+dp[i+1][1],dp[i+1][0]);
                }
            }
        }
        return dp[0][1];
    }
};
```
- Time complexity: O(n)
- Space complexity: O(n) (for the dp array)


### Space Optimization
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<int> prev(2,0);
        vector<int> temp(2,0);
        prev[0]=prev[1]=0;
        for(int i=n-1;i>=0;i--)
        {
            for(int buy=0;buy<=1;buy++)
            {
                if(buy)
                {
                    temp[buy] = max(-prices[i]+prev[0],prev[1]);
                }
                else
                {
                    temp[buy] = max(prices[i]+prev[1],prev[0]);
                }
            }
            prev=temp;
        }
        return prev[1];
    }
};
```
- Time complexity: O(n)
- Space complexity: O(1) (for the temp array)
- Here, we are using only two arrays of size 2, so we can optimize the space further by using only one array of size 2. We can use the same array to store the previous values and the current values.

