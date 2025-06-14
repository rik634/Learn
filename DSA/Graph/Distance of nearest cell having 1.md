# Distance of nearest cell having 1

- Given a binary grid of N*M. Find the distance of the nearest 1 in the grid for each cell.
- The distance is calculated as |i1  - i2| + |j1 - j2|, where i1, j1 are the row number and column number of the current cell, and i2, j2 are the row number and column number of the nearest cell having value 1.

### Logic
- As we are finding the the minimimum distance, for any nearest 1, we will go with BFS.

### BFS
```
vector<int> dx = {0,0,1,-1};
vector<int> dy = {1,-1,0,0};
vector<vector<int>> nearest(vector<vector<int>> grid)
{
    int n = grid.size();
    int m = grid[0].size();
    vector<vector<int>> dp(n,vector<int>(m,-1));
    queue<pair<pair<int,int>,int>> q;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<m;j++)
        {
            if(grid[i][j]==1)
            {
                q.push({{i,j},0});
            }
        }
    }
    while(!q.empty())
    {
        auto p = q.front();
        int x = p.first.first;
        int y = p.first.second;
        int d = p.second;
        q.pop();
        if(dp[x][y]==-1)
        {
            dp[x][y]=d;
        }
        for(int i=0;i<4;i++)
        {
            int r = x + dx[i];
            int c = y + dy[i];
            if(r>=0 && r<n && c>=0 && c<m && grid[r][c]==0 && dp[r][c]==INT_MAX)
            {
                q.push({{r,c},d+1});
            }
        }
    }
    return dp;
}
int main()
{
    vector<vector<int>> grid;
    nearest(grid);
    cout << grid;
}
```

- Time complexity : O(n*m) + O(n*m*4) = O(n*m)
- Space Complexity: O(n*m)
