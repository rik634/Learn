# Longest Common Subsequence

- Subsequence of a string can be obtained by picking 0 or all characters from it, and these characters should be picked in same order as they are in string
- Substring = we need to pick continuous characters.
- Subsequence = we need to pick some characters in same order but not the continuous ones.
- For a string of length n, there are total 2^n possible subsequence

### Naive solution: 
- Generate all the subsequence, and find the length of longest common subsequence.

### Recursive 
```
int lcs(string s1, string s2, int m , int n)
{
    if(m==0 || n==0)
    {
        return 0;
    }
    if(s1[m-1]==s2[n-2])
    {
        return 1 + lcs(s1,s2,m-1,n-1);
    }
    return max(lcs(s1,s2,m,n-1),lcs(s1,s2,m-1,n));
}
```
- Time complexity: O(2^n)

### Memoization
```
vector<vector<int>> dp(m,vector<int>(n,-1));
int lcs(string s1, string s2, int m, int n)
{
    if(dp[m][n]!=-1)
    {
        return dp[m][n];
    }
    if(m==0 || n==0)
    {
        dp[m][n]=0;
    }
    else if(s1[m-1]==s2[n-2])
    {
        dp[m][n]=1 + lcs(s1,s2,m-1,n-1);
    }
    else
    {
        dp[m][n]=max(lcs(s1,s2,m,n-1),lcs(s1,s2,m-1,n));

    }
    return dp[m][n];
}
```
- Time Complexity: theta(mn)
- Space Complexity: theta(mn)

### Tabulation
