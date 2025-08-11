# Best time to buy and sell stock

### Logic
- in case of stocks problem, space optimization is very important. We will be expected to give them optimized space code only.
- If you are selling on the i_th day, you have to buy on the day with minimum price from (1_st to (i-1)_th).

### Solution
```
int maxProfit(vector<int>& prices) {
        
        int mn=prices[0];
        int profit=0;
        for(int i=1;i<prices.size();i++)
        {
            int cost = prices[i]-mn;
            profit=max(profit,cost);
            mn=min(mn,prices[i]);
        }
        return profit;
    }
```
- Time complexity: O(N)
- space complexity: O(1)