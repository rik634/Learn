# Subset sum equal to target K
- You are given an array of N positive integers and an integer K. Your task is to check if there exists a subset with sum equal to K.
- Note: Return true if there exists a subset with sum equal to K. Otherwise, return false

### Logic:
- Subset might not need to follow the order in which elements are arranged. But, subsequence needs to follow the order.
- All subsequences are subsets. But, not all the subsets are the subsequences.
- As the basic approach, we generate all the subsequences and check if any of them gives a sum of K.
- There are 2 ways to generate the subsequences, one is power set and another is recursion.
- Here, if we observe, it is not asking to generate all the subsequences. We just need to check if there exists at least one subsequence which sums up to K.
-So, instead of generating all of them, we will follow the recursion method.
- Here, we have 2 parameters which will be changing, one being index, and another is target.
- There are 2 possibilities for any element of the array, one is it will part of subsequence and other is that it is not part of subset/subsequence.
- The function 'f(n-1,target)', it tells us in the array till the index, if for any one subsequence we have the sum equal to the target.

### Base case:
1. we achieve the target sum
2. we reach the last element of tree. i.e, the last element remaining is equal to the target

### POssible ways:
- Check if the value at that index is not greater than the target at that point.


### Recursion
```
int f(ind,target)
{
    if(target==0)
    {
        return true;
    }
    if(ind==0 )
    {
        return target==A[0];
    }
    
    bool notTake= f(ind-1,target);
    bool take=false;
    if(target>=A[ind])
    {
        take = f(ind-1,target-A[ind]);
    }
    
    return take|| notTake;
}
```
- Time Complexity: O(2^n) (At each index of the array of length N, we have 2 possible ways)
- Space Complexity: O(n) (recursive stack space)

- Here, if we observe the recursive tree, we get the overlapping subproblem. 
- Let's optimize it using the DP.
- here, there are 2 parameters which are changing the values, we need a 2D DP array.

### Memoization
```
vector<vector<int>> dp(N,vector<int>(target+1,-1));
int f(ind,target)
{
    if(target==0)
    {
        return dp[ind][target]=1;
    }
    if(ind==0 )
    {
        return dp[ind][target]=(target==A[0]);
    }
    if(dp[ind][target]!=-1)
    {
        return dp[ind][target];
    }
    int notTake= f(ind-1,target);
    int take=0;
    if(target>=A[ind])
    {
        take = f(ind-1,target-A[ind]);
    }
    
    return dp[ind][target]=take|| notTake;
}
```
- Time Complexity: O(N*target) 
- Space Complexity: O(N*target) (dp array) + O(n) (recursive stack space)

- Now, this recursive stack space can be reduced or optimized using the tabulation method

### Tabulation
```
vector<vector<int>> dp(N,vector<int>(target+1,0));
int f()
{
    for(int i=0;i<N;i++)
    {
        dp[i][0]=1;
    }
    dp[0][A[0]]=1;
    for(int i=1;i<N;i++)
    {
        for(int j=1;j<=target;j++)
        {
            int notTake = dp[i-1][j];
            int take=0;
            if(j>=A[i])
            {
                take = dp[i-1][j-A[i]];
            }
            dp[i][j]=notTake || take;
        }
    }
    return dp[N-1][target];
}
```
- Time Complexity: O(N*target) + O(N)
- Space Complexity; O(N*target)

### Space Optimized
```
int f()
{
    vector<int> prev(target+1,0);
    prev[0]=1;
    prev[A[0]]=1;
    for(int i=1;i<N;i++)
    {
        vector<int> temp(target+1,0);
        temp[0]=1;
        for(int j=1;j<=target;j++)
        {
            int notTake = prev[j];
            int take=0;
            if(j>=A[i])
            {
                take = prev[j-A[i]];
            }
            temp[j]=notTake || take;
        }
        prev=temp;
    }
    return prev[target];
}
```
- Time Complexity: O(N*target)
- Space Complexity: O(target)
