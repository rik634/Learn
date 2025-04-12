# Partition equal subset sum
- You are given an array of size N. Your task is to find if we can partition the given array into 2 subsets such that the sum of elements in both subsets is equal.

### Logic:
- As we can see, we need to partition into 2 subsets of equal sum. for each of the subsets, the sum has to be half of the sum of the whole array elements.
- Lets just make that sum half. and apply the function to find if that target sum is present in the subsets of array or not.

### Recursion
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
int main()
{
    vector<int> arr;
    int sum=0;
    for(int i=0;i<arr.size();i++)
    {
        sum+=arr[i];
    }
    if(sum%2!=0)
    {
        cout << false;
    }
    cout << f(arr.size()-1,sum/2);
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
int main()
{
    vector<int> arr;
    int sum=0;
    for(int i=0;i<arr.size();i++)
    {
        sum+=arr[i];
    }
    if(sum%2!=0)
    {
        cout << false;
    }
    cout << f(arr.size()-1,sum/2);
}
```
- Time Complexity: O(N*target)  + O(N)
- Space Complexity: O(N*target) (dp array) + O(n) (recursive stack space)

- Now, this recursive stack space can be reduced or optimized using the tabulation method


### Tabulation
```
vector<vector<int>> dp(N,vector<int>(target+1,0));
int f(vector<int> A)
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if(sum%2!=0)
    {
        return 0;
    }
    target=sum/2;
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
- Time Complexity: O(N*target) + O(N)+O(N)
- Space Complexity; O(N*target)


### Space optimized
```
int f(vector<int> A)
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if(sum%2!=0)
    {
        return 0;
    }
    target=sum/2;
    vector<int> prev(target+1,0);
    prev[0]=1;
    if(A[0]<=target)
    {
        prev[A[0]]=1;
    }
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
- Time Complexity: O(N*target) + O(N)
- Space Complexity: O(target)

