# Maze Obstacles
- Given a N*M maze with obstacles, count and return the number of paths to reach the right bottom cell from top left cell. A cell in the given maze has a value of -1 if it is a blockage or dead end, else 0. From a given cell, we are allowed to move to cells (i+1,j) and (i,j+1) only. Since the answer can be large, print it modulo 10^9+7.

### Recursion
```
int f(i,j)
{
    if(i>=0 && j>=0 && a[i][j]==-1)
    {
        return 0;
    }
    if(i==0 && j==0)
    {
        return 1;
    }
    if(i<0 || j<0)
    {
        return 0;
    }
    int up = f(i-1,j);
    int left = f(i,j-1);
    return up+left;
}
```
- Time Complexity: O(2^(m*n))
- Space Complexity: O(m+n-2 (path length)) (recursive stack space)

### Memoization
```
int mod = (int)(1e9+7);
vector<vector<int>> dp(m,vector<int>(n,-1));
int f(i,j)
{
    if(i>=0 && j>=0 && a[i][j]==-1)
    {
        return dp[i][j]=0;
    }
    if(i==0 && j==0)
    {
        return dp[i][j]=1;
    }
    if(i<0 || j<0)
    {
        return 0;
    }
    if(dp[i][j]!=-1)
    {
        return dp[i][j];
    }
    int up = f(i-1,j);
    int left = f(i,j-1);
    return dp[i][j]=(up+left)%mod;
}
```
- Time Complexity: O((m*n))
- Space Complexity: O(m+n-2 (path length)) (recursive stack space) + O(m*n) (array)

### Tabulation
```
int maze()
{
    vector<vector<int>> dp(m,vector<int>(n,-1));
    if(maze[0][0]==-1)
    {
        return 0;
    }
    dp[0][0]=1;
    for(int i=0;i<m;i++)
    {
        dp[i][0]= maze[i][0]==-1 ? 0 : 1;
    }
    for(int i=0;i<n;i++)
    {
        dp[0][i]=maze[0][i]==-1 ? 0 : 1;
    }
    for(int i=1;i<m;i++)
    {
        for(int j=1;j<n;j++)
        {
            if(maze[i][j]==-1)
            {
                dp[i][j]=0;
            }
            else
            {
                dp[i][j]=(dp[i-1][j]+dp[i][j-1])%mod;
            }
            
        }
    }
    return dp[m-1][n-1];
}

```
- Time Complexity: O(m*n) + O(m) + O(n)
- Space Complexity: O(m*n)
### Space Optimized

```
int f(int m, int n)
{
    vector<int> prev(n,0);
    if(maze[0][0]==-1)
    {
        return 0;
    }
    for(int i=1;i,n;i++)
    {
        prev[i]=maze[0][i]==-1 ? 0 : 1;
    }
    for(int i=1;i<m;i++)
    {
        vector<int> temp(n,-1);
        for(int j=0;j<n;j++)
        {
            if(maze[i][j]==-1)
            {
                temp[j]=0;
            }
            if(j==0)
            {
                temp[j]=1;
            }
            else
            {
                temp[j]=temp[j-1]+prev[j];
            }
        }
        prev=temp;
    }
    return prev[n-1];
}


```
- Time Complexity: O(m*n) 
- Space Complexity: O(n)
