# Triangle
- You are given a triangular array 'TRIANGLE'. Your task is to return the minimum path sum to reach from top to the bottom row.
- The triangle array will have N rows and the ith row, where 0<=i< N will have i+1 elements
- You can move only to the adjacent number of rows below each step. For example, if you are at index j in row i, then you can move to j or j+1 index in row i+1 in each step.


### Logic:
- Here, the starting point is fixed. The ending point can be any one of the last row indices.
- Here, as we need to find the minimum sum, we can either go for DP or greedy.
- Greedy, will work only when there is an uniformity in the values.
- While, in other case, we have to go for the DP solution. and Try all the possible solutions, and then find the minimum value.

- In general situation, where we have single source and single target/destination, we can find one recursion function and it will give the solution. But, in this case, we have multiple possible, destinations, and we need to find minimum sum from them.

- So, here, instead of finding the recursive solution for last index, it is preferable to go for top index first.


### Recursion
```
int f(i,j)
{
    if(i==n-1)
    {
        return A[i][j];
    }
    int down = A[i][j]+f(i+1,j);
    int diagonal = A[i][j]+f(i+1,j+1);
    return min(down,diagonal);
}

```
- Time Complexity: O(2^(n*(n+1)/2))
- Space Complexity: O(n) (recursive stack space)

### Memoization
```
vector<vector<int>> dp(n,vector<int>(n,INT_MAX));
int f(i,j)
{
    if(i==n-1)
    {
        return dp[i][j]=A[i][j];
    }
    if(dp[i][j]!=-1)
    {
        return dp[i][j];
    }
    int down = A[i][j]+f(i+1,j);
    int diagonal = A[i][j]+f(i+1,j+1);
    return dp[i][j]= min(down,diagonal);
}

```
- Time Complexity: O(n*n)
- Space Complexity: O(n) (recursive stack space) + O(n*n) (array)

### Tabulation

```
vector<vector<int>> dp(n,vector<int>(n,INT_MAX));
int f()
{
    for(int i=0;i<n;i++)
    {
        dp[n-1][i]=A[n-1][j];
    }
    for(int i=n-2;i>=0;i--)
    {
        
        for(int j=i;j>=0;j--)
        {
            int up = A[i][j]+dp[i+1][j];
            int dg= A[i][j]+dp[i+1][j+1];
            dp[i][j]=min(up,dg);
        }
    }
    return dp[0][0];
}

```
- Time Complexity: O(n*n)
- Space Complexity: O(n*n)

### Space Optimized
```
int f ()
{
    vector<int> prev(n,0);
    for(int i=0;i<n;i++)
    {
        prev[i]=A[n-1][i];
    }
    for(int i=n-2;i>=0;i--)
    {
        vector<int> temp(n,0);
        for(int j=i;j>=0;j--)
        {
            int up = A[i][j]+prev[j];
            int dg= A[i][j]+prev[j+1];
            temp[j]=min(up,dg);
        }
        
        prev=temp;
    }
    return prev[0];
}
```
- Time Complexity: O(n*n)
- Space Complexity: O(n)
