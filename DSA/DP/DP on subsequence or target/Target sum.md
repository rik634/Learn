# Target Sum
- We are given an array ‘ARR’ of size ‘N’ and a number ‘Target’. Our task is to build an expression from the given array where we can place a ‘+’ or ‘-’ sign in front of an integer. We want to place a sign in front of every integer of the array and get our required target. We need to count the number of ways in which we can achieve our required target.

### Logic
- It is same as the problem: "count partitions with given difference".


### Recursion
```
int f(ind, target)
{
    if(target==0)
    {
        return 1;
    }
    if(ind==0)
    {
        return target==A[ind] ? 1 : 0;
    }
    int notTake = f(ind-1,target);
    int take=0;
    if(A[ind]<=target)
    {
        take = f(ind-1,target-A[ind]);
    }
    return (notTake + take);
}
int main()
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if((sum-D)<0 || (sum-D)%2!=0)
    {
        cout << false;
    }
    f(N-1, (sum-D)/2);
}
```
- Time Complexity: O(2^N)
- Space Complexity: O(N) (Recursive stack space)



### Memoization

```
vector<vector<int>> dp(n,vector<int>(target+1,-1));
int f(ind, target)
{
    if(target==0)
    {
        return dp[ind][target]= 1;
    }
    if(ind==0)
    {
        return dp[ind][target]=target==A[ind] ? 1 : 0;
    }
    if(dp[ind][target]!=-1)
    {
        return dp[ind][target];
    }
    int notTake = f(ind-1,target);
    int take=0;
    if(A[ind]<=target)
    {
        take = f(ind-1,target-A[ind]);
    }
    return dp[ind][target]=(notTake + take);
}
int main()
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if((sum-D)<0 || (sum-D)%2!=0)
    {
        cout << false;
    }
    f(N-1, (sum-D)/2);
}
```
- Time Complexity: O(N*target)
- Space Complexity: O(N*target) +O(N) (Recursive stack space)


### Tabulation
```
vector<vector<int>> dp(n,vector<int>(target+1,0));
int f()
{
    for(int i=0;i<N;i++)
    {
        dp[i][0]=1;
    }
    if(A[0]<=target)
    {
        dp[0][A[ind]]=1;
    }
    for(int i=1;i<N;i++)
    {
        for(int j=1;j<=target;j++)
        {
            int notTake = dp[i-1][j];
            int take=0;
            if(A[i]<=j)
            {
                take = dp[i-1][j-A[i]];
            }
            dp[i][j]=(notTake + take);
        }
    }
    return dp[N-1][target];
}
int main()
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if((sum-D)<0 || (sum-D)%2!=0)
    {
        cout << false;
    }
    f(N-1, (sum-D)/2);
}

```
- Time Complexity: O(N) + O(N*target)
- Space Complexity: O(N*target)



### Space Optimized

```
int f()
{
    vector<int> prev(target+1,0);
    prev[0]=1;
    if(A[0]<=target)
    {
        prev[A[ind]]=1;
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(target+1,0);
        temp[0]=1;
        for(int j=1;j<=target;j++)
        {
            int notTake = prev[j];
            int take=0;
            if(A[i]<=j)
            {
                take = prev[j-A[i]];
            }
            temp[j]=(notTake + take);
        }
        prev=temp;
    }
    return prev[target];
}
int main()
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if((sum-D)<0 || (sum-D)%2!=0)
    {
        cout << false;
    }
    f(N-1, (sum-D)/2);
}

```
- Time Complexity: O(N) + O(N*target)
- Space Complexity: O(target)


## Constraints:
- A[i] can be 0 as well

### Tabulation:
```
vector<vector<int>> dp(n,vector<int>(target+1,0));
int f()
{
    if(A[0]==0)
    {
        dp[0][0]=2;
    }
    else
    {
        dp[0][0]=1;
    }
    if(A[0]!=0 && A[0]<=target)
    {
        dp[0][A[0]]=1;
    }
    for(int i=1;i<N;i++)
    {
        for(int j=1;j<=target;j++)
        {
            int notTake = dp[i-1][j];
            int take=0;
            if(A[i]<=j)
            {
                take = dp[i-1][j-A[i]];
            }
            dp[i][j]=(notTake + take);
        }
    }
    return dp[N-1][target];
}
int main()
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if((sum-D)<0 || (sum-D)%2!=0)
    {
        cout << false;
    }
    f(N-1, (sum-D)/2);
}

```
- Time Complexity: O(N) + O(N*target)
- Space Complexity: O(N*target)

### Space optimized

```
int f()
{
    vector<int> prev(target+1,0);

    if(A[0]==0)
    {
        prev[0]=2;
    }
    else
    {
        prev[0]=1;
    }
    if(A[0]!=0 && A[0]<=target)
    {
        prev[A[0]]=1;
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(target+1,0);
        for(int j=1;j<=target;j++)
        {
            int notTake = prev[j];
            int take=0;
            if(A[i]<=j)
            {
                take = prev[j-A[i]];
            }
            temp[j]=(notTake + take);
        }
        prev=temp
    }
    return prev[target];
}
int main()
{
    int sum=0;
    for(int i=0;i<N;i++)
    {
        sum+=A[i];
    }
    if((sum-D)<0 || (sum-D)%2!=0)
    {
        cout << false;
    }
    f(N-1, (sum-D)/2);
}

```
- Time Complexity: O(N) + O(N*target)
- Space Complexity: O(target)
