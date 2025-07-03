# Wildcard Matching
- Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

- '?' Matches any single character.
- '*' Matches any sequence of characters (including the empty sequence).
- The matching should cover the entire input string (not partial).

### Logic
- f(i,j): returns if the s1(0,i) matches with s2(0,j).
- for the string matching with '*', we have to try to match all possible strings. IF any one string gets matched, we will return true. Or else, we will return false.
- As we are getting all possible ways, we go for recursion.
- In the case, where '*' is present in string, we try all possible ways like this: f(i-1,j) || f(i,j-1).

### Steps/rules to write recursion:
- express in (i,j)
- Explore comparisons
- Out of all comparisons, is anyone can. If yes, return true. or else return false.

### Recursion
```
int f(i,j)
{
    if(i<0 && j<0)
    {
        return true;
    }
    if(i<0 && j>=0)
    {
        return false;
    }
    if(j<0 && i>=0)
    {
        for(int k=i;k>=0;k--)
        {
            if(s1[k]!='*')
            {
                return false;
            }
        }
        return true;
    }
    if(s1[i]==s2[j] || s1[i ]=='?)
    {
        return f(i-1,j-1);
    }
    if(s1[i]=='*')
    {
        return f(i-1,j) || f(i,j-1);
    }
    return false;
}

```
- Time Complexity: exponential
- Space Complexity: O(N+M) (recursive stack space)

- We will check in recursion tree, if there are overlapping sub problems. In such case, we go for memoization.
- in memoization, max value of i= N, max value of j=M. so, we will need dp[N][M];

### Memoization
```
class Solution {
public:
    bool f(int i,int j, string& s, string& p, vector<vector<int>>& dp)
    {
        if(i<0 && j<0)
        {
            return true;
        }
        if(i<0 && j>=0)
        {
             for(int k=j;k>=0;k--)
            {
                if(p[k]!='*')
                {
                    return false;
                }
            }
            return true;
            
        }
        if(j<0 && i>=0)
        {
           return false;
        }
        if(dp[i][j]!=-1)
        {
            return dp[i][j];
        }
        if(s[i]==p[j] || p[j]=='?')
        {
            return dp[i][j]=f(i-1,j-1,s,p,dp);
        }
        if(p[j]=='*')
        {
            return dp[i][j]=f(i-1,j,s,p,dp) || f(i,j-1,s,p,dp);
        }
        return dp[i][j]=false;
    }

    bool isMatch(string s, string p) {
        int n = s.length();
        int m = p.length();
        vector<vector<int>> dp(n,vector<int>(m,-1));
        return f(n-1,m-1,s,p,dp);
    }
};
```
- Time complexity: O(N*M) (because, we will be doing the computation, only these many times)
- Space Complexity: O(N*M) (for dp array) + O(N*M) (auxiliary stack space)

### Memoization with 1-based indexing, so it will be easy to convert to tabulation solution
```
class Solution {
public:
    bool f(int i,int j, string& s, string& p, vector<vector<int>>& dp)
    {
        if(i==0 && j==0)
        {
            return true;
        }
        if(i==0 && j>0)
        {
             for(int k=j;k>0;k--)
            {
                if(p[k-1]!='*')
                {
                    return false;
                }
            }
            return true;
            
        }
        if(j==0 && i>0)
        {
           return false;
        }
        if(dp[i][j]!=-1)
        {
            return dp[i][j];
        }
        if(s[i-1]==p[j-1] || p[j-1]=='?')
        {
            return dp[i][j]=f(i-1,j-1,s,p,dp);
        }
        if(p[j-1]=='*')
        {
            return dp[i][j]=f(i-1,j,s,p,dp) || f(i,j-1,s,p,dp);
        }
        return dp[i][j]=false;
    }

    bool isMatch(string s, string p) {
        int n = s.length();
        int m = p.length();
        vector<vector<int>> dp(n+1,vector<int>(m+1,-1));
        return f(n,m,s,p,dp);
    }
};
```

### Tabulation
```
class Solution {
public:
    
    bool isMatch(string s, string p) {
        int n = s.length();
        int m = p.length();
        vector<vector<bool>> dp(n+1,vector<bool>(m+1,false));
        dp[0][0]=true;
        for(int j=1;j<=m;j++)
        {
            dp[0][j]=true;
            for(int k=1;k<=j;k++)
            {
                if(p[k-1]!='*')
                {
                    dp[0][j]=false;
                    break;
                }
            }
           
        }
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=m;j++)
            {
                if(s[i-1]==p[j-1] || p[j-1]=='?')
                {
                     dp[i][j]=dp[i-1][j-1];
                }
                else if(p[j-1]=='*')
                {
                    dp[i][j]=dp[i-1][j] || dp[i][j-1];
                }
            }
        }
        return dp[n][m];
    }
};
```
- Time complexity: O(N*M)
- Space Complexity: O(N*M)

### Space Optimized
```
class Solution {
public:
    
    bool isMatch(string s, string p) {
        int n = s.length();
        int m = p.length();
        vector<bool> prev(m+1,false);
        prev[0]=true;
        for(int j=1;j<=m;j++)
        {
            prev[j]=true;
            for(int k=1;k<=j;k++)
            {
                if(p[k-1]!='*')
                {
                    prev[j]=false;
                    break;
                } 
            }
           
        }
        for(int i=1;i<=n;i++)
        {
            vector<bool> temp(m+1,false);
            for(int j=1;j<=m;j++)
            {
                if(s[i-1]==p[j-1] || p[j-1]=='?')
                {
                     temp[j]=prev[j-1];
                }
                else if(p[j-1]=='*')
                {
                    temp[j]=prev[j] || temp[j-1];
                }
            }
            prev=temp;
        }
        return prev[m];
    }
};
```
- Time complexity: O(N*M)
- space complexity: O(M)
