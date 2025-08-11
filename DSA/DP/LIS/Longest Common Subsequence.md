# Longest Common Subsequence

- Subsequence of a string can be obtained by picking 0 or all characters from it, and these characters should be picked in same order as they are in string
- Substring = we need to pick continuous characters.
- Subsequence = we need to pick some characters in same order but not the continuous ones.
- For a string of length n, there are total 2^n possible subsequence

### Naive solution: 
- Generate all the subsequence, and find the length of longest common subsequence.

### Recursive 
```cpp
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
```cpp
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

```cpp
int lcs(string s1, string s2)
{
    int m = s1.length();
    int n = s2.length();
    vector<vector<int>> dp(m+1,vector<int>(n+1,0));
    for(int i=0;i<=m;i++)
    {
        dp[i][0]=0;
    }
    for(int i=0;i<=n;i++)
    {
        dp[0][i]=0;
    }
    for(int i=1;i<=m;i++)
    {
        for(int j=1;j<=n;j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                dp[i][j]=1+dp[i-1][j-1];
            }
            else
            {
                dp[i][j]=max(dp[i][j-1],dp[i-1][j]);
            }
        }
    }
    return dp[m][n];
}
```
- Time Complexity: theta(m)+theta(n)+theta(mn)=theta(mn)
- Space Complexity: theta(mn)

## Variations of LCS:
1. Diff utility( difference between 2 files) :
   -  find the common part in 2 files using LCS, and remove the part which is not common from the first file
   - used in version control systems like GitLab, GitHub,and extra in 2nd file is added than in the first file
2. Minimum insertions and deletions to convert s1 into s2:
   - answer = 1 insertion and 1 deletion
   - m-l deletions and n-l insertions.
   - L = length of common subsequence
   - M = length of 1st string
   - N = length of 2nd string

3.	Shortest common supersequence (find shortest such a string which has given 2 strings as subsequence):
   - Find the LCS, traverse first string and insert in the middle extra characters to LCS. similarly in case of 2nd string.

4.	Longest palindromic subsequence:
   - Take this given string as s1, and s2 as reverse of this string.
   - Find the LCS

5.	Longest repeating subsequence:
-	We need to consider the characters only at the different indexes.
-	S1 is s and s2 is also s.  
-	Change the condition from LCS such that 2 characters match but their indices don’t match.

6.	Space optimized DP solution of LCS
-	Instead of using 2d array if size (m+1)*(n+1), we can use 2*n or 2*m.

7.	Printing LCS
-	Create a string and append the character to the string when characters match. Use 2d array