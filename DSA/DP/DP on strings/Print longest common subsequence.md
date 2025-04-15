# Print longest common subsequence
- We are given 2 strings, s1 and s2. Print the longest common subsequence.
### Logic
- We calculate the length of LCS. And, to get the longest common subsequence string, we use backtracking by making use of the length we calculated.

### Tabulation
```
vector<vector<int>> dp(n+1,vector<int>(m+1,0));
string f()
{
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                dp[i][j]=1+dp[i-1][j-1];
            }
            else
            {
                dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
    }
    int len = dp[n][m];
    string ans="";
    for(int i=0;i<len;i++)
    {
        ans+="$";
    }
    int index=len-1;
    int i=n,j=m;
    while(i>0 && j>0)
    {
        if(s1[i-1]==s2[j-1])
        {
            ans[index]=s1[i-1];
            index--;
            i--;
            j--;
        }
        else if(dp[i-1][j]>dp[i][j-1])
        {
            i--;
        }
        else
        {
            j--;
        }
    }
    return ans;
}
```
- Time Complexity: O(n*m) + O(n+m)(backtracking)
- Space Complexity: O(n*m)
