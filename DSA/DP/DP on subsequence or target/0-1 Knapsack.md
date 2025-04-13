# 0-1 Knapsack

- A thief wants to rob a store. He is carrying a bag of capacity W. The store has ‘n’ items. Its weight is given by the ‘wt’ array and its value by the ‘val’ array. He can either include an item in its knapsack or exclude it but can’t partially have it as a fraction. We need to find the maximum value of items that the thief can steal.

### Logic
- Here, we can either use DP or greedy.
- Here, greedy will not work, due to law of uniformity. In order for us to use greedy, the values should be uniform, i.e., increasing or decreasing.
- so, we will go with the DP approach.
- f(n-1,W): Till the index N-1, what is the maximum value that can be generated, given the size of the bag is W.

### Steps:
1. Express in terms of (ind,W)
2. Explore all the possibilities (take, notTake)
3. Max(all possibilities) 

### Recursion
```
int f(ind, W)
{
   
    if(ind==0)
    {
        return (wt[ind]<=W) ? val[ind] : 0;
    }
    int notTake = f(ind-1,W);
    int take = INT_MIN;
    if(wt[ind]<=W)
    {
        take = val[ind] + f(ind-1,W-wt[ind]);
    }
    return max(notTake, take);
}
```
- Time Complexity: O(2^N)
- Space Complexity: O(N) (Recursive stack space)

### Memoization
```
vector<vector<int>> dp(N,vector<int>(W+1,-1));
int f(ind, W)
{
    if(ind==0)
    {
        return dp[ind][W]= (wt[ind]<=W) ? val[ind] : 0;
    }
   
    if(dp[ind][W]!=-1)
    {
        return dp[ind][W];
    }
    int notTake = f(ind-1,W);
    int take =0;
    if(wt[ind]<=W)
    {
        take = val[ind] + f(ind-1,W-wt[ind]);
    }
    return dp[ind][W]=max(notTake, take);
}
```
- Time Complexity: O(N*W)
- Space Complexity: O(N*W) (array) +O(N) (Auxiliary stack space)

### Tabulation
```
vector<vector<int>> dp(N,vector<int>(W+1,0));
int f()
{
    for(int i=wt[0];i<=W;i++)
    {
        dp[0][i]=val[0];
    }
    for(int i=1;i<N;i++)
    {
        for(int j=1;j<=W;j++)
        {
            int notTake = dp[i-1][j];
            int take =INT_MIN;
            if(wt[i]<=j)
            {
                take = val[i] + dp[i-1][j-wt[i]];
           }
           dp[i][j]=max(notTake, take);
        }
    }
    return dp[N-1][W];
}
```
- Time Complexity: O(W) + O(N*W)
- Space Complexity: O(N*W)

### Space Optimized (2 1D array optimization)
```
int f()
{
    vector<int> prev(W+1,0);
    for(int i=wt[0];i<=W;i++)
    {
       prev[i]=val[0];
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(W+1,0);
        for(int j=1;j<=W;j++)
        {
            int notTake = prev[j];
            int take =INT_MIN;
            if(wt[i]<=j)
            {
                take = val[i] + prev[j-wt[i]];
           }
           temp[j]=max(notTake, take);
        }
        prev=temp;
    }
    return prev[W];
}
```
- Time Complexity: O(W) + O(N*W)
- Space Complexity: O(2*W)


### More Space Optimized (1, 1D array optimization)
```
int f()
{
    vector<int> prev(W+1,0);
    for(int i=wt[0];i<=W;i++)
    {
       prev[i]=val[0];
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(W+1,0);
        for(int j=W;j>=1;j--)
        {
            int notTake = prev[j];
            int take =INT_MIN;
            if(wt[i]<=j)
            {
                take = val[i] + prev[j-wt[i]];
           }
           prev[j]=max(notTake, take);
        }
    }
    return prev[W];
}
```
- Time Complexity: O(W) + O(N*W)
- Space Complexity: O(W)

