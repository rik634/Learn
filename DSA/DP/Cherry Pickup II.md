# Cherry pickup II
- You are given a rows x cols matrix grid representing a field of cherries where grid[i][j] represents the number of cherries that you can collect from the (i, j) cell.

- You have two robots that can collect cherries for you:

1. Robot #1 is located at the top-left corner (0, 0), and
2. Robot #2 is located at the top-right corner (0, cols - 1).

- Return the maximum number of cherries collection using both robots by following the rules below:

1. From a cell (i, j), robots can move to cell (i + 1, j - 1), (i + 1, j), or (i + 1, j + 1).
2. When any robot passes through a cell, It picks up all cherries, and the cell becomes an empty cell.
3. When both robots stay in the same cell, only one takes the cherries.
4. Both robots cannot move outside of the grid at any moment.
5. Both robots should reach the bottom row in grid.
 


### Logic:
- Here,finding the max sum path for both the robots separately will not work, as we might miss out on some of the common elements.
- Here, we need to find the max sum path for both together.
- Here, we have fixed starting point for both the robots. In such case, we write the recursion from the starting point instead of the ending point.
- Also, both the robots will reach the destination or the last row at the same time. This is because, if we observe the steps that are allowed for the robot, at each step, it is going to the next down row, which answers the query.
- As a result, the row index for the co-ordinates of the both robots is same, as considered as only one parameter.
- Here, for every movement of Alice, we have 3 possible movements for the Bob.
- So, at each step, we have 9 possible combination moves for Alice and Bob.
- For each (i,j), next possible steps are (i+1,j-1), (i+1,j), (i+1,j+1).




### Base case:
- Base can be either the destination or the out of boundary.

### Steps:
1. Express everything in terms of (i1,j1) and (i2,j2)
2. Explore all the paths (down, down-left, down-right)
3. Max sum

### Recursion
```
int f(i,j1,j2)
{
    if(j1<0 || j1>M ||  j2<0 || j2>M)
    {
        return -1e9;
    }
    if(i==N-1)
    {
        if(j1==j2)
        {
            return A[i][j1];
        }
        else
        {
            return A[i][j1]+A[i][j2];
        }
    }
    int mx=INT_MIN;
    for(int a=-1;a<=1;a++)
    {
        for(int b=-1;b<=1;b++)
        {
            if(j1==j1)
            {
                mx=max(mx,A[i][j1]+f(i+1,j1+a,j2+b));
            }
            else
            {
                mx=max(mx,A[i][j1]+A[i][j2]+f(i+1,j1+a,j2+b));
            }
        }
    }
    return mx;
}
```
- Time Complexity: (3^n)*(3^n) (There are 3 possible next steps for each of both alice and bob)
- Space Complexity: O(2*n)

- Here, we have overlapping subproblem. It can be optimized using memoization. 
- We have 3 changing parameters, i, j1, and j2. 
- max value for i = N
- max value for j1= M
- max value for j2 = M
- So, dp vector will be a 3D vector with above sizes.

### Memoization
```
int dp[N][M][M];
int f(i,j1,j2)
{
    if(j1<0 || j1>=M ||  j2<0 || j2>=M)
    {
        return -1e9;
    }
    if(i==N-1)
    {
        if(j1==j2)
        {
            return dp[i][j1][j2]= A[i][j1];
        }
        else
        {
            return dp[1][j1][j2]=A[i][j1]+A[i][j2];
        }
    }
    if(dp[i][j1][j2]!=-1)
    {
        return dp[i][j1][j2];
    }
    int mx=INT_MIN;
    for(int a=-1;a<=1;a++)
    {
        for(int b=-1;b<=1;b++)
        {
            if(j1==j1)
            {
                mx=max(mx,A[i][j1]+f(i+1,j1+a,j2+b));
            }
            else
            {
                mx=max(mx,A[i][j1]+A[i][j2]+f(i+1,j1+a,j2+b));
            }
        }
    }
    return dp[i][j1][j2]=mx;
}
```
- Time Complexity: O(N*M*M)*9 (multiplied by 9, as for every state in array, we are running the for loop to go to next step, which is running 9 times, as 9 is a significant number, can not ignore it)
- Space Complexity: O(N*M*M) (Array) + O(N) (Auxiliary stack space) 


### Tabulation
```
vector<vector<vector<int>>> dp(N,vector<vector<int>>(M,vector<int>(M,INT_MIN)));
int f()
{
    for(int j1=0;j1<M;j1++)
    {
        for(int j2=0;j2<M;j2++)
        {
            if(j1==j2)
            {
                dp[N-1][j1][j2]=A[N-1][j2];
            }
            else
            {
                dp[N-1][j1][j2]=A[N-1][j1]+A[N-1][j2];
            }
        }
    }
    for(i=N-2;i>=0;i--)
    {
        for(int j1=M-1;j1>=0;j1--)
        {
            for(int j2=M-1;j2>=0;j2--)
            {
                int mx=INT_MIN;
                for(int a=-1;a<=1;a++)
               {
                    for(int b=-1;b<=1;b++)
                   {
                        if(j1==j1)
                        {
                            mx=max(mx,A[i][j1]+dp[i+1][j1+a][j2+b]);
                        }
                        else
                        {
                            mx=max(mx,A[i][j1]+A[i][j2]+dp[i+1][j1+a][j2+b]);
                       }
                   }
               }
               dp[i][j1][j2]=mx;
            }
        }
    }
    return dp[0][0][M-1];
}

```
- Time Complexity: O(M*M) + O(N*M*M)*9
- Space Complexity: O(N*M*M)

- space optimization
1. 1D dp array can be space optimized to 2 variables
2. 2D array to 1D Dp
3. 3D array to 2D Dp


### Space Optimized
```
int maximumChocolates(int n, int m, vector<vector<int>> &grid) {
    
    vector<vector<int>> front(m, vector<int>(m, 0));
    vector<vector<int>> cur(m, vector<int>(m, 0));

    for (int j1 = 0; j1 < m; j1++) {
        for (int j2 = 0; j2 < m; j2++) {
            if (j1 == j2)
                front[j1][j2] = grid[n - 1][j1];
            else
                front[j1][j2] = grid[n - 1][j1] + grid[n - 1][j2];
        }
    }

    for (int i = n - 2; i >= 0; i--) {
        for (int j1 = 0; j1 < m; j1++) {
            for (int j2 = 0; j2 < m; j2++) {
                int maxi = INT_MIN;
                for (int di = -1; di <= 1; di++) {
                    for (int dj = -1; dj <= 1; dj++) {
                        int ans;

                        if (j1 == j2)
                            ans = grid[i][j1];
                        else
                            ans = grid[i][j1] + grid[i][j2];

                        if ((j1 + di < 0 || j1 + di >= m) || (j2 + dj < 0 || j2 + dj >= m))
                            ans += -1e9; 
                        else
                            ans += front[j1 + di][j2 + dj]; 
                        maxi = max(ans, maxi); 
                    }
                }
                cur[j1][j2] = maxi;
            }
        }
        front = cur; 
    }
    return front[0][m - 1];
}
```
- Time Complexity: O(M*M) + O(N*M*M)*9
- Space Complexity: O(M*M)