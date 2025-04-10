# Grid Unique Paths (DP on grids or 2D matrix)
- You are present at point 'A' which is the top-left cell of an M*N matrix, your destination point is 'B', which is the bottom-right cell of the same matrix. Your task is to find the total number of unique paths from point 'A' to point 'B'. In other words, you will be given the dimensions of the matrix as integers 'M' and 'N'. Your task is to find the total number of unique paths from the cell A to B.
- To traverse in the matrix, you can either move right or down at each step

### Logic:
- We are asked to find unique paths. Here, we need to find all the paths, and get unique ones from them.
- So, as we need to find all possible ways, we go for recursion.
- Get a recursive solution. Check the recursion tree, if we have the overlapping subproblem.
- To optimize this, we go for DP.

### Steps:
1. Express in terms of indices
2. Explore/do all stuffs
3. Sum up all ways/ max/ min

### Recursion
```
int f(i,j)
{
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
- Time Complexity: (2^(m*n)) (m=rows, n=col. Here, we have m*n states, for each states, 2 possibilities)
- Space Complexity: O(m+n-2(max path length)) (Recursive stack space)

- Here, if we observer recursion tree, we get overlapping subproblem. So, we go for DP.

### Memoization

```
vector<vector<int>> dp(m,vector<int>(n,-1));
int f(i,j)
{
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
    return dp[i][j]=up+left;
}

```
- Time Complexity: O(m*n)
- Space Complexity: O(m+n-2(path length)) (stack space)+ O(m*n)(array) = O(m*n)

## Memoization to Tabulation
1. Declare base case
2. Express all states in for loop
3. Copy the recurrence and write

### Tabulation
```
vector<vector<int>> dp(m,vector<int>(n,-1));
int f(int m, int n)
{
    d[0][0]=1;
    for(int i=1;i<m;i++)
    {
        dp[i][0]=1;
    }
    for(int i=1;i<n;i++)
    {
        dp[0][i]=1;
    }
    for(int i=1;i<m;i++)
    {
        for(int j=1;j<n;j++)
        {
            dp[i][j]=dp[i-1][j]+dp[i][j-1];
        }
    }
    return dp[m-1][n-1];
}
```
- Time Complexity: O(m*n) + O(m) + O(n) = O(m*n)
- Space Complexity: O(m*n)

- If there is a previous row or previous column, we can space optimize it.

### Space Optimized
```
int f(int m, int n)
{
    vector<int> prev(n,1);
    for(int i=1;i<m;i++)
    {
        vector<int> temp(n,-1);
        for(int j=0;j<n;j++)
        {
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