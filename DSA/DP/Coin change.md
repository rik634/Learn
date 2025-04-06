# Coin Change
- We are given a array of types of coins available and the sum.
- We have to find the total number of ways to get the sum, using given coin types.
- We have infinite supple of each type of coin.

### Recursive:
```
int getCount(int coins[], int n, int sum)
{
    if(sum==0)
    {
        return 1;
    }
    if(n==0)
    {
        return 0;
    }
    int res = getCount(coins, n-1, sums); //Here, we are not considering the coin type at that index
    if(coins[n-1]<=sum)
    {
        res = res + getCount(coins, n, sum-coins[n-1]); // considering that coin type
    }
    return res;
}
```
- Time Complexity: O(n)

### Tabulation
- In recursive solution, 2 parameters n and sum are changing. So, we create a 2-dimensional dp array of size (sum+1)*(n+1).
- dp[i][j] indicate the number of combinations that we can get for sum=i, and considering the coins from 1 to j.
- When the value of parameters in edge cases is possible to be 0, we will create the dp array with size parameter-
- We can’t keep the dimension of 2d array as (n+1)*(sum+1), as it will not work.
```
int getCount(int coins[], int n, int sum)
{
    vector<vector<int>> dp(sum+1,vector<int>(n+1,0));
    for(int i=0;i<=n;i++)
    {
        dp[0][i]=1;
    }
    for(int i=1;i<=sum;i++)
    {
        dp[i][0]=0;
    }
    for(int i=1;i<=sum;i++)
    {
        for(int j=1;j<=n;j++)
        {
            dp[i][j]=dp[i][j-1];
            if(coins[j-1]<=i)
            {
                dp[i][j]+=dp[i-coins[j-1]][j];
            }
        }
    }
    return dp[sum][n];
}
```

- Time complexity = theta(n*sum)
- Space complexity = theta(n*sum)
- Space optimized solution = space complexity = theta(sum)




