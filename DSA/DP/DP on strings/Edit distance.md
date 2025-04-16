# Edit Distance
- Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:
1. Insert a character
2. Delete a character
3. Replace a character
 
### Logic
- Here, by comparing the strings, we need to try all possible ways, and get the minimum operations from them.

### Recursion
```
int f(i,j)
{
    if(i<0) // when s1 gets exhausted, we can have one or more than one characters remaining in s2. So, need to perform insert operation for that.
    {
        return (j+1);
    }
    if(j<0) // when s2 gets exhausted, in order to convert s1 to s2, we need to delete all the characters remaining in s1
    {
        return (i+1);
    }
    
    if(s1[i]==s2[j])
    {
        return f(i-1,j-1);
    }
    int insert = 1 + f(i,j-1);
    int delete = 1 + f(i-1,j);
    int replace = 1 + f(i-1,j-1);
    return max(insert,max(delete,replace));
}
```
- Time Complexity: ((3^n)*(3^m))
- Space Complexity: O(n+m) (Auxiliary stack space)

### Memoization
```
vector<vector<int>> dp(n,vector<int>(m,-1));
int f(i,j)
{
    if(i<0) // when s1 gets exhausted, we can have one or more than one characters remaining in s2. So, need to perform insert operation for that.
    {
        return (j+1);
    }
    if(j<0) // when s2 gets exhausted, in order to convert s1 to s2, we need to delete all the characters remaining in s1
    {
        return (i+1);
    }
    if(dp[i][j]!=-1)
    {
        return dp[i][j];
    }
    if(s1[i]==s2[j])
    {
        return dp[i][j]=f(i-1,j-1);
    }
    int insert = 1 + f(i,j-1);
    int delete = 1 + f(i-1,j);
    int replace = 1 + f(i-1,j-1);
    return dp[i][j]=max(insert,max(delete,replace));
}
```
- Time Complexity: (n*m)
- Space Complexity: O(n*m)+ O(n+m) (Auxiliary stack space)

- To get the Tabulation, as the base case is -1, we have to shift the indexes.
### Tabulation
 ```
vector<vector<int>> dp(n+1,vector<int>(m+1,0));
int f()
{
    for(int i=0;i<=n;i++)
    {
        dp[i][0]=i;
    }
    for(int j=1;j<=m;j++)
    {
        dp[0][j]=j;
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                dp[i][j]=dp[i-1][j-1];
            }
            else
            {
                int insert = 1 + dp[i][j-1];
                int delete = 1 + dp[i-1][j];
                int replace = 1 + dp[i-1][j-1];
                dp[i][j]=max(insert,max(delete, replace));
            }
        }
    }
    return dp[n][m];
}
 ```
- Time Complexity: O(n) + O(m) + O(n*m)
- Space Complexity : O(n*m)

### Space Optimized
 ```
int f()
{
    vector<int> prev(m+1,0);
    for(int j=1;j<=m;j++)
    {
        prev[j]=j;
    }
    for(int i=1;i<=n;i++)
    {
        vector<int> temp(m+1,0);
        temp[0]=i;
        for(int j=1;j<=m;j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                temp[j]=prev[j-1];
            }
            else
            {
                int insert = 1 + temp[j-1];
                int delete = 1 + prev[j];
                int replace = 1 + prev[j-1];
                temp[j]=max(insert,max(delete, replace));
            }
        }
        prev=temp;
    }
    return prev[m];
}
 ```
- Time Complexity: O(m) + O(n*m)
- Space Complexity : O(m)

