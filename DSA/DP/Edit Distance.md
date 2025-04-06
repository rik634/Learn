# Edit Distance
- Here we are given 2 strings, s1 and s2. We need to convert s1 into s2.
- We are allowed to do the operations insert, delete and replace.
- We need to do this conversion using minimum number of operations

### Recursive solution

```
int editDistance(string s1, string s2, int m , int n)
{
    if(m==0)
    {
        return n;
    }
    if(n==0)
    {
        return m; 
    }
    if(s1[m-1]==s2[n-1])
    {
        return editDistance(s1,s2,m-1,n-1);
    }
    return 1+min(editDistance(s1,s2,m,n-1),editDistance(s1,s2,m-1,n),editDistance(s1,s2,m-1,n-2));
}
```
- Time Complexity: O(2^n)
### Tabulation

```
int editDistance(string s1, string s2, int m , int n)
{
    int dp[m+1][n+1];
    for(int i=0;i<=m;i++)
    {
        dp[i][0]=i;
    }
    for(int j=0;j<=n;j++)
    {
        dp[0][j]=j;
    }
    for(int i=1;i<=m;i++)
    {
        for(int j=1;j<=n;j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                dp[i][j]=dp[i-1][j-1];
            }
            else
            {
                dp[i][j]=min(dp[i][j-1],dp[i-1][j],dp[i-1][j-1]);
            }
        }
    }
    return dp[m][n];
}
```
- Time Complexity: theta(mn)
- Space Complexity: theta(mn)


## Applications:
1.	Suggesting spelling errors
-	If we write spelling of a word wrong, we find the closest word in dictionary.
-	When we type any word, we can suggest the words with edit distance of 1 or 2 with the given word.
2.	Used in bioinformatics

