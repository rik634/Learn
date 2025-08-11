# Best time to buy and sell stock with cooldown
- We are given an array Arr[] of length n. It represents the price of a stock on ‘n’ days. The following guidelines need to be followed:

   1. We can buy and sell the stock any number of times.
   2. In order to sell the stock, we need to first buy it on the same or any previous day.
   3. We can’t buy a stock again after buying it once. In other words, we first buy a stock and then sell it. After selling we can buy and sell again. But we can’t sell before buying and can’t buy before selling any previously bought stock.
   4. We can’t buy a stock on the very next day of selling it. This is the cooldown clause.


### Logic


### Recursion
```
 int f(int ind, int buy, vector<int>& prices)
    {
        if(ind>=prices.size())
        {
            return 0;
        }
        int profit=0;
        if(buy)
        {
            profit = max(-prices[ind]+f(ind+1,0,prices),f(ind+1,1,prices));
        }
        else
        {
            profit = max(prices[ind]+f(ind+2,1,prices),f(ind+1,0,prices));
        }
        return profit;
    }
    int maxProfit(vector<int>& prices) {
        
        int n = prices.size();
    
        return f(0,1,prices);
    }
```
- Time complexity: O(2^n) (for each day, we have two choices: buy or not buy)
- Space complexity: O(n) (for the recursion stack)

### Memoization
``` 
 int f(int ind, int buy, vector<int>& prices,vector<vector<int>>& dp)
    {
        if(ind>=prices.size())
        {
            return 0;
        }
        int profit=0;
        if(dp[ind][buy]!=-1)
        {
            return dp[ind][buy];
        }
        if(buy)
        {
            profit = max(-prices[ind]+f(ind+1,0,prices,dp),f(ind+1,1,prices,dp));
        }
        else
        {
            profit = max(prices[ind]+f(ind+2,1,prices,dp),f(ind+1,0,prices,dp));
        }
        return dp[ind][buy]=profit;
    }
    int maxProfit(vector<int>& prices) {
        
        int n = prices.size();
        vector<vector<int>> dp(n,vector<int>(2,-1));
        return f(0,1,prices,dp);
    }
```
- Time complexity: O(n * 2) = O(n) (since we are storing the results of subproblem)
- Space complexity: O(n) (auxiliary space for recursion stack) + O(n * 2) = O(n) (for the dp array)

### Tabulation
```
 int maxProfit(vector<int>& prices) {
        
        int n = prices.size();
        vector<vector<int>> dp(n+2,vector<int>(2,0));
        for(int ind=n-1;ind>=0;ind--)
        {
            for(int buy=0;buy<=1;buy++)
            {
                int profit=0;
                if(buy)
                {
                    profit = max(-prices[ind]+dp[ind+1][0],dp[ind+1][1]);
                }
                else
                {
                    profit = max(prices[ind]+dp[ind+2][1],dp[ind+1][0]);
                }
                dp[ind][buy]=profit;
            }
        }
        return dp[0][1];
    }
```
- Time complexity: O(n * 2) = O(n) (since we are storing the results of subproblem)
- Space complexity: O(n) (auxiliary space for recursion stack) + O(n * 2) = O(n) (for the dp array) 

### Space Optimized Solution
```
  int maxProfit(vector<int>& prices) {
        
        int n = prices.size();
        vector<int> prev1(2,0);
        vector<int> prev2(2,0);
        vector<int> temp(2,0);
        for(int ind=n-1;ind>=0;ind--)
        {
            for(int buy=0;buy<=1;buy++)
            {
                int profit=0;
                if(buy)
                {
                    profit = max(-prices[ind]+prev1[0],prev1[1]);
                }
                else
                {
                    profit = max(prices[ind]+prev2[1],prev1[0]);
                }
                temp[buy]=profit;
            }
            prev2=prev1;
            prev1=temp;
        }
        return prev1[1];
    }
```
- Time complexity: O(n*2) (since we are iterating through the prices array)
- Space complexity: O(2*3) (we are using only a constant amount of space for variables)


### more optimized
```
int maxProfit(vector<int>& prices) {
        
        int n = prices.size();
        vector<int> prev1(2,0);
        vector<int> prev2(2,0);
        vector<int> temp(2,0);
        for(int ind=n-1;ind>=0;ind--)
        {
            temp[1] = max(-prices[ind]+prev1[0],prev1[1]);
            temp[0] = max(prices[ind]+prev2[1],prev1[0]);
                
            
            prev2=prev1;
            prev1=temp;
        }
        return prev1[1];
    }
```
- Time complexity: O(n) (since we are iterating through the prices array)
- Space complexity: O(2) (we are using only a constant amount of space for variables)   
