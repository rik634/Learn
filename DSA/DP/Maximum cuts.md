# Maximum Cuts
- We are given length of rod as n and the values a, b, and c.
- We want to make maximum cuts in this rod such that length of cut will be either a or b or c.
- If we are not able to get cuts if given lengths, return -1

### Recursive:
```
int maxCuts(int n, int a , int b, int c)
{
    if(n<0)
    {
        return -1;
    }
    if(n==0)
    {
        return 0;
    }
    int res = max(maxCuts(n-a,a,b,c),maxCuts(n-b,a,b,c),maxCuts(n-c,a,b,c));
    if(res==-1)
    {
        return res;
    }
    return res+1;
}
```
- Time Complexity: O(3^n)


### Tabulation:
```
int maxCuts(int n, int a, int b, int c)
{
    int dp[n+1];
    dp[0]=0;
    for(int i=1;i<=n;i++)
    {
        dp[i]=-1;
        if(i-a>=0)
        {
            dp[i]=max(dp[i],dp[i-a]);
        }
         if(i-b>=0)
        {
            dp[i]=max(dp[i],dp[i-b]);
        } if(i-c>=0)
        {
            dp[i]=max(dp[i],dp[i-c]);
        }
        if(dp[i]!=-1)
        {
            dp[i]++;
        }
    }
    return dp[n];
}
```
- Time Complexity: theta(n)
