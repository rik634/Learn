# Frog Jump
- There is a frog on the 1st step of an N stairs long staircase. The frog wants to reach the Nth stair. height[i] is the height of the (i+1)th stair. If the frog jumps from ith to jth stair, the energy lost in the jump is given by (height[i-1]-height[j-1]). If the frog is on i th staircase, he can jump either to (i+1)th stair or to (i+2)th stair. Find the minimum total energy used by frog to reach from 1st stair to Nth stair.

### Recursion
```
int jump(int n)
{
    if(n==0)
    {
        return 0;
    }
    if(n==1)
    {
        return abs(h[1]-h[0]);
    }
    int l = jump(n-1)+abs(h[n]-h[n-1]);
    int r = jump(n-2)+abs(h[n]-h[n-2]);
    return min(l,r);
}
```
- Time Complexity: O(2^n)

### Memoization

```
vector<int> dp(n+1,-1);
int jump(int n)
{
    if(dp[n]!=-1)
    {
        return dp[n];
    }
    if(n==0)
    {
        return dp[0]=0;
    }
    if(n==1)
    {
        return dp[1]=abs(h[1]-h[0]);
    }
    int l = jump(n-1)+abs(h[n]-h[n-1]);
    int r = jump(n-2)+abs(h[n]-h[n-2]);
    return dp[n]=min(l,r);
}
```
- Time Complexity: O(N)
- Space Complexity: O(N) (Recursion stack space) + O(N) (Dp array) = O(N)

### Tabulation

```
int jump(int n)
{
    vector<int> dp(n+1,-1);
    dp[0]=0;
    dp[1]=abs(h[1]-h[0]);
    for(int i=2;i<=n;i++)
    {
        dp[i] = min(dp[i-1]+abs(h[i]-h[i-1]),dp[i-1]+abs(h[i]-h[i-2]));
    }
    return dp[n];
}
```
- Time Complexity: O(N)
- Space Complexity: O(N) (Dp array)

### Space optimization
```
int jump(int n)
{
    int prev1=0;
    int prev2=abs(h[1]-h[0]);
    int temp;
    for(int i=2;i<=n;i++)
    {
        temp = min(prev2+abs(h[i]-h[i-1]),prev1 +abs(h[i]-h[i-2]));
        prev1=prev2;
        prev2=temp;
    }
    return temp;
}
```
- Time Complexity: O(N)
- Space Complexity: O(1) 


## Follow-up: instead of one or two jumps, try for K jumps. Value of K will be given


### Recursion
```
int jump(int n)
{
    if(n==0)
    {
        return 0;
    }
    mnSteps=INT_MAX;
    for(int j=1;j<=k;j++)
    {
        if((n-j)>=0)
        {
            int jump = jump(n-j)+abs(h[n]-h[n-j]);
            mnSteps=min(mnSteps,jump);
        }
    }
    return mnSteps;
}
```
- Time Complexity: O(k^n)

### Memoization
```
vector<int> dp(n+1,INT_MAX);
int jump(int n)
{
    if(dp[n]!=INT_MAX)
    {
        return dp[n];
    }
    if(n==0)
    {
        return dp[0]=0;
    }
    mnSteps=INT_MAX;
    for(int j=1;j<=k;j++)
    {
        if((n-j)>=0)
        {
            int jump = jump(n-j)+abs(h[n]-h[n-j]);
            mnSteps=min(mnSteps,jump);
        }
    }
    return dp[n]=mnSteps;
}
```
- Time Complexity: O(N)
- Space Complexity: O(N) (DP array) + O(N)(Recursion stack space) = O(N)

### Tabulation
```
int jump(int n)
{
    vector<int> dp(n+1,INT_MAX);
    dp[0]=0;
    dp[1]=abs(h[1]-h[0]);
    for(int i=2;i<=n;i++)
    {
        for(int j=1;j<=k;j++)
        {
           if((i-j)>=0)
           {
             dp[i]=min(dp[i],dp[i-j]+abs(h[i]-h[i-j]));
           }
        }
    }
    return dp[n];
}
```

- Time Complexity: O(K*N)
- Space Complexity: O(N)

### Space Optimized

- Here, Space complexity will be O(K), as we need to store K values in variables. Here, we can use array of size K, to store the last K values.

