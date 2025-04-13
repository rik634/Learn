# Coin change 2
- We are given an array Arr with N distinct coins and a target. We have an infinite supply of each coin denomination. We need to find the number of ways we sum up the coin values to give us the target.
- Each coin can be used any number of times.

### Logic
- Infinite supply, stay at same index for pick condition, instead of reducing by 1.
- Count ways: sum the possibilities.


### Recursion
```
int f(int,target)
{
    if(ind==0)
    {
        return target%A[0]==0;
    }
    int notTake = f(ind-1, target);
    int take=0;
    if(A[ind]<=target)
    {
        take = f(ind,target-A[ind]);
    }
    return notTake + take.
}
```
- Time Complexity; O(2^N)
- Space Complexity: O(target)

### Memoization
```
vector<vector<int>> dp(N,vector<int>(target+1,-1));
int f(int,target)
{
    if(ind==0)
    {
       return dp[ind][target] = (target%A[0]==0);
    }
    if(dp[ind][target]!=-1)
    {
        return dp[ind][target];
    }
    int notTake = f(ind-1, target);
    int take=0;
    if(A[ind]<=target)
    {
        take = f(ind,target-A[ind]);
    }
    return dp[ind][target]=notTake + take.
}
```
- Time Complexity; O(N*target)
- Space Complexity: O(target) + O(N*target)

### Tabulation
```
vector<vector<int>> dp(N,vector<int>(target+1,0));
int f()
{

    for(int i=0;i<=target;i++)
    {
        dp[0][i]= (i%A[0]==0);
    }
    for(int i=1;i<N;i++)
    {
        for(int j=0;j<=target;j++)
        {
            int notTake = dp[i-1][j];
            int take=0;
            if(A[i]<=j)
            {
                take = dp[i][j-A[i]];
            }
            return dp[i][j]=notTake + take.
        }
    }
    return dp[N-1][target];
}
```
- Time Complexity: O(target) + O(N*target)
- Space Complexity: O(N*target) 

### Space Optimized
```

int f()
{
   vector<int> prev(target+1,0);
   for(int i=0;i<=target;i++)
    {
        prev[i]= (i%A[0]==0);
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(target+1,0);
        for(int j=0;j<=target;j++)
        {
            int notTake = prev[j];
            int take=0;
            if(A[i]<=j)
            {
                take = temp[j-A[i]];
            }
            temp[j]=notTake + take.
        }
        prev=temp;
    }
    return prev[target];
}
```
- Time Complexity: O(target) + O(N*target)
- Space Complexity: O(target) 

