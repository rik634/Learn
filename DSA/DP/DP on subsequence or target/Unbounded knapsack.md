# Unbounded knapsack
- A thief wants to rob a store. He is carrying a bag of capacity W. The store has ‘n’ items of infinite supply. Its weight is given by the ‘wt’ array and its value by the ‘val’ array. He can either include an item in its knapsack or exclude it but can’t partially have it as a fraction. We need to find the maximum value of items that the thief can steal. He can take a single item any number of times he wants and put it in his knapsack.

### Logic
- Infinite supply = for pick condition, use same index instead of reducing it by one.

### Recursion
```
int f(ind,W)
{
    if(ind==0)
    {
        return (int)(W/wt[0])*val[0];
    }
    int notTake = f(ind-1,W);
    int take=INT_MIN;
    if(wt[ind]<=W)
    {
        take = val[ind] + f(ind,W-wt[ind]);
    }
    return max(notTake, take);
}
```
- Time Complexity: O(2^N)
- Space Complexity: >>O(N), and in worst case, it will be = O(W) (when we are reducing the capacity by one each at each step)

### Memoization
```
vector<vector<int>> dp(N,vector<int>(W+1,-1));
int f(ind,W)
{
    if(ind==0)
    {
       return dp[ind][W]=(int)(W/wt[0])*val[0];
    }
    if(dp[ind][W]!=-1)
    {
        return dp[ind][W];
    }
    int notTake = f(ind-1,W);
    int take=INT_MIN;
    if(wt[ind]<=W)
    {
        take = val[ind] + f(ind,W-wt[ind]);
    }
    return dp[ind][W]=max(notTake, take);
}
```
- Time Complexity: O(N8W)
- Space Complexity: >>O(N), and in worst case, it will be = O(target) + O(N*W)

### Tabulation
```
vector<vector<int>> dp(N,vector<int>(W+1,INT_MIN));
int f()
{
    for(int i=1;i<=W;i++)
    {
        dp[0][i]=(int)(i/wt[0])*val[0];
    }
    for(int i=1;i<N;i++)
    {
        for(int j=0;j<=W;j++)
        {
            int notTake = dp[i-1][j];
            int take=INT_MIN;
            if(wt[i]<=j)
            {
                take = val[ind] + dp[i][j-wt[i]];
            }
            dp[i][j]=max(notTake, take);
        }
    }
    return dp[N-1][W];
}
```
- Time Complexity: O(N*W) + O(W)
- Space Complexity: O(N*W)

### Space Optimized
```
int f()
{
    vector<int> prev(W+1,INT_MIN);
    for(int i=1;i<=W;i++)
    {
        prev[i]=(int)(i/wt[0])*val[0];
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(W+1,INT_MIN);
        for(int j=0;j<=W;j++)
        {
            int notTake = prev[j];
            int take=INT_MIN;
            if(wt[i]<=j)
            {
                take = val[ind] + temp[j-wt[i]];
            }
            temp[j]=max(notTake, take);
        }
        prev=temp;
    }
    return prev[W];
}
```
- Time Complexity: O(N*W) + O(W)
- Space Complexity: O(2*W)

### More optimized
```
int f()
{
    vector<int> prev(W+1,INT_MIN);
    for(int i=1;i<=W;i++)
    {
        prev[i]=(int)(i/wt[0])*val[0];
    }
    for(int i=1;i<N;i++)
    {
        
        for(int j=W;j>=0;j--)
        {
            int notTake = prev[j];
            int take=INT_MIN;
            if(wt[i]<=j)
            {
                take = val[ind] + prev[j-wt[i]];
            }
            prev[j]=max(notTake, take);
        }
    }
    return prev[W];
}
```
- Time Complexity: O(N*W) + O(W)
- Space Complexity: O(W)