# Maximum/ Minimum Falling Path Sum
- You have been given an N*M matrix filled with integer numbers, find the maximum sum that can be obtained from a oath starting from any cell in the first row to any cell in the last row.
- From a cell in a row, you can move to another cell directly, below that row, or diagonally below left or right. So, from a particular cell row (row,col), we can move in the three directions etc.
--> Down (row=1,col)
--> Down left diagonal: (row+1,col-1)
--> Down right diagonal: (row+1,col+1)


### Logic:
- Here, we have variable both source and destination.
- Here, we can't solve this problem by greedy, due to the absence of uniformity in the values. As uniformity is not there, we don't know, how the values are spread across.
- So, here, we will try out all the paths, and take the best one of them. 
- To try out all the paths, we go with recursion.

### Base case:
- A base can be either destination or the out of bound case.

### Recursion
```
int f(i,j)
{
    if(j<0 || j>=n)
    {
        return -1e9;
    }
    if(i==0)
    {
        return A[i][j];
    } 
    
    int u = A[i][j]+f(i-1,j);
    int ul =A[i][j]+f(i-1,j-1);
    int ur = A[i][j]+f(i-1,j+1);
    return max(u,max(ul,ur));

}
```
- Time Complexity: O(3^n) (n=path length, 3=possible ways for each index pair)
- Space Complexity: O(n)

### Memoization
```
vector<vector<int>> dp(N,vector<int>(M,-1e9));
int f(i,j)
{
    if(j<0 || j>=n)
    {
        return -1e9;
    }
    if(i==0)
    {
        return A[i][j];
    } 
    if(dp[i][j]!=-1e9)
    [
        return dp[i][j];
    ]
    int u = A[i][j]+f(i-1,j);
    int ul =A[i][j]+f(i-1,j-1);
    int ur = A[i][j]+f(i-1,j+1);
    return dp[i][j]=max(u,max(ul,ur));

}
```
- Time Complexity: O(N*M) 
- Space Complexity: O(N*M)(array) + O(N)(recursive stack space)


### Tabulation
```
vector<vector<int>> dp(N,vector<int>(M,-1e9));
int f()
{
    for(int i=0;i<M;i++)
    {
        dp[0][i]=A[0][i];
    }
    for(int i=1;i<N;i++)
    {
        int u=-1e9;
        int ul=-1e9;
        int ur=-1e9;
        for(int j=0;j<M;j++)
        {
            u = A[i][j]+dp[i-1][j];
            if(j>=1)
            {
                ul=A[i][j]+dp[i-1][j-1];
            }
            if((j+1)<M)
            {
                ur = A[i][j]+dp[i-1][j+1];
            }
            dp[i][j]=max(u,max(ul,ur));
        }
    }
    int mx=INT_MIN;
    for(int i=0;i<M;i++)
    {
        mx=max(mx,dp[N-1][i]);
    }
    return mx;
}
```
- Time Complexity: O(M)+ O(N*M) + O(M) = O(M)
- Space Complexity: O(N*M)

### Space Optimized
```
int f()
{
    vector<int> prev(M,0);
    for(int i=0;i<M;i++)
    {
        prev[i]=A[0][i];
    }
    for(int i=1;i<N;i++)
    {
        vector<int> temp(M,0);
        int u=-1e9;
        int ul=-1e9;
        int ur=-1e9;
        for(int j=0;j<M;j++)
        {
            u = A[i][j]+prev[j];
            if(j>=1)
            {
                ul=A[i][j]+prev[j-1];
            }
            if((j+1)<M)
            {
                ur = A[i][j]+prev[j+1];
            }
            temp[j]=max(u,max(ul,ur));
        }
        prev=temp;
    }
    int mx=INT_MIN;
    for(int i=0;i<M;i++)
    {
        mx=max(mx,prev[i]);
    }
    return mx;
}
```
- Time Complexity: O(M) + O(N*M) + O(M)
- Space Complexity: O(M)
