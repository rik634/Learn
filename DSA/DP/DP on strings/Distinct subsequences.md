# Distinct subsequences
- Given two strings s and t, return the number of distinct subsequences of s which equals t.

- The test cases are generated so that the answer fits on a 32-bit signed integer.

### Logic
- Here, we are asked about the number/ count of ways, for some given condition. So, for the base case, it will either return 1 or 0.
- And we sum the possible operations/ways and return.
- When there are 2 strings, we have 2 parameters for the recurrence function.
- f(n-1,m-1): Number of subsequences of s2(0,m-1) in s1(0,n-1). 
- When we return1=> If all characters of S2 match, then return 1. all characters match mean, the index for S2, i.e., J becomes -1.
- If j>=0, but i=-1, then we still have s2 characters remaining to be matched, but characters for s1 are no more remaining. Then we return 0.

### Steps for recursion:
1. Express everything in terms of (i,j) index.
2. Explore all possibilities
3. Return summation of all possibilities
4. Base case

### Recursion
```
int f(i,j)
{
    if(j<0)
    {
        return 1;
    }
    if(i<0)
    {
        return 0;
    }
    if(s1[i]==s2[j])
    {
        return f(i-1,j-1)+f(i-1,j);
    }
    return f(i-1,j);
}
int main()
{
    string s1, s2;
    cout << f(n-1,m-1);
}
```
- Time Complexity: O((2^n)*(2^m))
- Space Complexity: O(n+m)

### Memoization
```
vector<vector<int>> dp(n,vector<int>(m,-1));
int f(i,j)
{
    if(j<0)
    {
        return 1;
    }
    if(i<0)
    {
        return 0;
    }
    if(dp[i][j]!=-1)
    {
        return dp[i][j];
    }
    if(s1[i]==s2[j])
    {
        return dp[i][j]=f(i-1,j-1)+f(i-1,j);
    }
    return dp[i][j]=f(i-1,j);
}
```
- Time Complexity: O(n*m)
- Space Complexity: O(n+m) + O(n+m) (auxiliary stack space)

- For tabulation, for base case, index is going <0.So, we will shift the indexes by 1.
### Tabulation
```
vector<vector<double>> dp(n+1,vector<double>(m+1,0));
int f()
{
    for(int i=0;i<n;i++)
    {
        dp[i][0]=1;
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                dp[i][j]=dp[i-1][j-1]+dp[i-1][j];
            }
            else
            {
                dp[i][j]=dp[i-1][j];
            }
        }
    }
    return dp[n][m];
}
```
- Time Complexity: O(n*m) + O(n)
- Space Complexity: O(n*m)

### Space Optimized
```
int f()
{
    vector<int> prev(m+1,0);
    prev[0]=1;
    for(int i=1;i<=n;i++)
    {
        vector<int> temp(m+1,0);
        temp[0]=1;
        for(int j=1;j<=m;j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                temp[j]=prev[j-1]+prev[j];
            }
            else
            {
                temp[j]=prev[j];
            }
        }
        prev=temp;
    }
    return prev[m];
}
```
- Time Complexity: O(n*m) + O(n)
- Space Complexity: O(2*m)

### More Space Optimized
```
int f()
{
    vector<int> prev(m+1,0);
    prev[0]=1;
    for(int i=1;i<=n;i++)
    {
        for(int j=m;j>=0;j+-)
        {
            if(s1[i-1]==s2[j-1])
            {
                prev[j]=prev[j-1]+prev[j];
            }
        }
    }
    return prev[m];
}
```
- Time Complexity: O(n*m) + O(n)
- Space Complexity: O(m)