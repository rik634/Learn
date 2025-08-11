# Best time to buy and sell stock IV
-  We are given an array Arr[] of length n. It represents the price of a stock on ‘n’ days. The following guidelines need to be followed:

   1. We can buy and sell the stock any number of times.
   2. In order to sell the stock, we need to first buy it on the same or any previous day.
   3. We can’t buy a stock again after buying it once. In other words, we first buy a stock and then sell it. After selling we can buy and sell again. But we can’t sell before buying and can’t buy before selling any previously bought stock.
   4. We can do at-most K transactions.


### Logic

### Recursion
```
int f(int ind, int buy, int cap, vector<int>& prices)
    {
        if(cap==0)
        {
            return 0;
        }
        if(ind==prices.size())
        {
            return 0;
        }
        int profit=0;
        if(buy)
        {
            profit = max(-prices[ind]+f(ind+1,0,cap,prices),f(ind+1,1,cap,prices));
        }
        else
        {
            profit = max(prices[ind]+f(ind+1,1,cap-1,prices),f(ind+1,0,cap,prices));
        }
        return profit;
    }
    int maxProfit(int k, vector<int>& prices) {
        
        return f(0,1,k,prices);
    }
```
- Time Complexity:O(2^n) (for each day, we have two choices: buy or not buy)
- Space Complexity:O(n) (for the recursion stack)

### Memoization
``` 
int f(int ind, int buy, int cap, vector<int>& prices,vector<vector<vector<int>>>& dp)
    {
        if(cap==0)
        {
            return 0;
        }
        if(ind==prices.size())
        {
            return 0;
        }
        if(dp[ind][buy][cap]!=-1)
        {
            return dp[ind][buy][cap];
        }
        int profit=0;
        if(buy)
        {
            profit = max(-prices[ind]+f(ind+1,0,cap,prices,dp),f(ind+1,1,cap,prices,dp));
        }
        else
        {
            profit = max(prices[ind]+f(ind+1,1,cap-1,prices,dp),f(ind+1,0,cap,prices,dp));
        }
        return dp[ind][buy][cap]=profit;
    }
    int maxProfit(int k,vector<int>& prices) {
        
        int n = prices.size();
        vector<vector<vector<int>>> dp(n,vector<vector<int>>(2,vector<int>(k+1,-1)));
        return f(0,1,k,prices,dp);
    }
```
- Time Complexity:O(n * 2 * k) (since we are storing the results of subproblem)
- Space Complexity:O(n * 2 * k) + O(n) (auxiliary stack space ) = O(n) (for the dp array)  


### Tabulation
```
int maxProfit(int k, vector<int>& prices) {
          int n = prices.size();
         vector<vector<vector<int>>> dp(n+1,vector<vector<int>>(2,vector<int>(k+1,0)));
         for(int i=0;i<=n;i++)
         {
            for(int buy=0;buy<=1;buy++)
            {
                dp[i][buy][0]=0;
            }
         }
         for(int buy=0;buy<=1;buy++)
         {
            for(int cap=0;cap<=k;cap++)
            {
                dp[n][buy][cap]=0;
            }
         }
         for(int ind=n-1;ind>=0;ind--)
         {
            for(int buy=0;buy<=1;buy++)
            {
                for(int cap=1;cap<=k;cap++)
                {
                    int profit=0;
                    if(buy)
                    {
                        profit = max(-prices[ind]+dp[ind+1][0][cap],dp[ind+1][1][cap]);
                    }
                    else
                    {
                        profit = max(prices[ind]+dp[ind+1][1][cap-1],dp[ind+1][0][cap]);
                    }
                    dp[ind][buy][cap]=profit;
                }
            }
         }
         return dp[0][1][k];
    }
```
- Time complexity:O(n * 2 * k) (since we are storing the results of subproblem)
- Space complexity:O(n * 2 * k) (for the dp array)  

### Space optimization
```
int maxProfit(int k, vector<int>& prices) {
             
         int n = prices.size();
        vector<vector<int>> prev(2,vector<int>(k+1,0));
        vector<vector<int>> temp(2,vector<int>(k+1,0));

         for(int ind=n-1;ind>=0;ind--)
         {
            for(int buy=0;buy<=1;buy++)
            {
                for(int cap=1;cap<=k;cap++)
                {
                    int profit=0;
                    if(buy)
                    {
                        profit = max(-prices[ind]+prev[0][cap],prev[1][cap]);
                    }
                    else
                    {
                        profit = max(prices[ind]+prev[1][cap-1],prev[0][cap]);
                    }
                    temp[buy][cap]=profit;
                }
            }
            prev=temp;
         }
         return prev[1][k];
    }
```

- Time complexity:O(n * k) (since we are storing the results of subproblem)
- Space complexity:O(k) (for the dp array)


- In order to further optimize this, instead of buy and cap variables, we can use a single variable to represent the state of the transaction, like the variable transaction can have the values from 0 to 3. As we want only 2 (B-S). so, there will be 2 B and 2 S transactions. Here, for the values of the variable, 0 and 2 represent thr buy state, and 1 and 3 represent the sell state. So, we can use a single variable to represent the state of the transaction.

### Space Optimized Solution with Single Transaction Variable
### recursion
```
int f(int ind, int tran,vector<int>& prices,int k)
    {
        if(ind==prices.size() || tran==2*k)
        {
            return 0;
        }
        int profit=0;
        if(tran%2==0) // buy
        {
            profit = max(-prices[ind]+f(ind+1,tran+1,prices,k),f(ind+1,tran,prices,k));
        }
        else // sell
        {
            profit = max(prices[ind]+f(ind+1,tran+1,prices,k),f(ind+1,tran,prices,k));
        }
        return profit;
    }
    int maxProfit(int k, vector<int>& prices) {
        
        int n = prices.size();
        return f(0,0,prices,k);
    }
```
- time complexity: O(2^n) (for each day, we have two choices: buy or not buy)
- space complexity: O(n) (for the recursion stack)


### Memoization
```
int f(int ind, int tran,vector<int>& prices,vector<vector<int>>& dp,int k)
    {
        if(ind==prices.size() || tran==2*k)
        {
            return 0;
        }
        int profit=0;
        if(dp[ind][tran]!=-1)
        {
            return dp[ind][tran];
        }
        if(tran%2==0) // buy
        {
            profit = max(-prices[ind]+f(ind+1,tran+1,prices,dp,k),f(ind+1,tran,prices,dp,k));
        }
        else // sell
        {
            profit = max(prices[ind]+f(ind+1,tran+1,prices,dp,k),f(ind+1,tran,prices,dp,k));
        }
        return dp[ind][tran]=profit;
    }
    int maxProfit(int k, vector<int>& prices) {
        
        int n = prices.size();
        vector<vector<int>> dp(n,vector<int>(2*k,-1));
        return f(0,0,prices,dp,k);
    }
```
- time complexity: O(n * 2*k) = O(n) (since we are storing the results of subproblem)
- space complexity: O(n * 2*k) + O(n) (auxiliary space for recursion stack) = O(n) (for the dp array)

### Tabulation
```
 int maxProfit(int k, vector<int>& prices) {
        
         int n = prices.size();
        vector<vector<int>> dp(n+1,vector<int>(2*k+1,0));
        for(int i=n-1;i>=0;i--)
        {
            for(int tran=2*k-1;tran>=0;tran--)
            {
                int profit=0;
                if(tran%2==0) // buy
                {
                    profit = max(-prices[i]+dp[i+1][tran+1],dp[i+1][tran]);
                }
                else // sell
                {
                    profit = max(prices[i]+dp[i+1][tran+1],dp[i+1][tran]);
                }
                dp[i][tran]=profit;
            }
        }
        return dp[0][0];
    }
```
- Time complexity: O(n * 2*k) = O(n) (since we are storing the results of subproblem)
- Space complexity: O(n * 2*k) = O(n) (for the dp array)

### Space Optimized
```
int maxProfit(int k, vector<int>& prices) {
        
        int n = prices.size();
        vector<int> prev(2*k+1,0);
        vector<int> temp(2*k+1,0);
        for(int ind=n-1;ind>=0;ind--)
        {
            for(int tran=2*k-1;tran>=0;tran--)
            {
                int profit=0;
                if(tran%2==0) // buy
                {
                    profit = max(-prices[ind]+prev[tran+1],prev[tran]);
                }
                else // sell
                {
                    profit = max(prices[ind]+prev[tran+1],prev[tran]);
                }
                temp[tran]=profit;
            }
            prev=temp;
        }
        return prev[0];
    }
```
- Time complexity: O(n * 2*k) = O(n) (since we are storing the results of subproblem)
- Space complexity: O(2*k+1) = O(1) (for the dp array)  
