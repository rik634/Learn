# Minimum Path Sum
- We are given a grid with N rows and M columns. Each box is associated with some cost. Find path from top left to bottom right box, which minimizes the sum of cost of all numbers along the path. Return the minimum sum of the path.
- we can only move down or right.

### Recursion
```
int f(i,j)
{
    if(i==0 && j==0)
    {
        return grid[i][j];
    }
    if(i<0 || j<0)
    {
        return INT_MAX;
    }
    int up = f(i-1,j)+grid[i][j];
    int left = f(i,j-1)+grid[i][j];
    return min(up,left);
}
```
- Time Complexity: O(2^(N*M))
- Space Complexity: O(N+M-2 (path length)) (recursive stack space)

### Memoization
```
vector<vector<int>> dp(N,vector<int>(M,INT_MAX));
int f(i,j)
{
    if(i==0 && j==0)
    {
        return dp[i][j]=grid[i][j];
    }
    if(i<0 || j<0)
    {
        return INT_MAX;
    }
    if(dp[i][j]!=INT_MAX)
    {
        return dp[i][j];
    }
    int up = f(i-1,j)+grid[i][j];
    int left = f(i,j-1)+grid[i][j];
    return dp[i][j]=min(up,left);
}
```
- Time Complexity: O(N*M)
- Space Complexity: O(N*M)(Array) + O(N+M-2) (Recursive stack space)

### Tabulation
```
vector<vector<int>> dp(N,vector<int>(M,INT_MAX));
int f(vector<vector<int>> grid,int N, INT M)
{
    dp[0][0]=grid[0][0];
    for(int i=1;i<N;i++)
    {
        dp[i][0]=dp[i-1]+grid[i][0];
    }
    for(int i=1;i<M;i++)
    {
        dp[0][i]=dp[0][i-1]+grid[0][i];
    }
    for(int i=1;i<N;i++)
    {
        for(int j=1;j<M;j++)
        {
            dp[i][j]=grid[i][j]+min(dp[i-1][j],dp[i][j-1]);
        }
    }
    return dp[N-1][M-1];
}
```
- Time Complexity: O(N*M)
- Space Complexity: O(N*M)

### Space Optimized
```

int f(vector<vector<int>> grid,int N, INT M)
{
    vector<int> prev(M,0);
    prev[0]=grid[0][0];
    for(int i=1;i<M;i++)
    {
        prev[i]=prev[0]+grid[0][i];
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(M,0);
        int up=INT_MAX;
        int left = INT_MAX;
        for(int j=0;j<M;j++)
        {
            up = prev[j];
            if(j>0)
            {
                down = temp[j-1];
            }
            temp[j]= grid[i][j]+min(up,down);
        }
        prev=temp;
    }
    return prev[M-1];
}
```
- Time Complexity: O(N*M)
- Space Complexity: O(M)

