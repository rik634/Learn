# Number of Enclaves
- You are given an N x M binary matrix grid, where 0 represents a sea cell and 1 represents a land cell. A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid. Find the number of land cells in the grid for which we cannot walk off the boundary of the grid in any number of moves.

### Logic
- Its similar to the 'surrounded regions' problem.
- We can use Multi-source BFS. 

### BFS
```
int main()
{
    vector<vector<int>> grid;
    int n = grid.size();
    int m = grid[0].size();
    vector<vector<bool>> vis(n,vector<bool>(m,false));
    queue<pair<int,int>> q;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<m;j++)
        {
            if(grid[i][j]==1 && (i==0 || j==0 || i==n-1 || j==m-1))
            {
                q.push({i,j});
            }
        }
    }
    vector<int> dx = {0,0,1,-1};
    vector<int> dy = {1,-1,0,0};
    while(!q.empty())
    {
        auto p = q.front();
        int x = p.first;
        int y = p.second;
        q.pop();
        for(int i=0;i<4;i++)
        {
            int r = x + dx[i];
            int c = y + dy[i];
            if(r>=0 && r<n && c>=0 && c<m && grid[r][c]==1 && vis[r][c]=false)
            {
                vis[r][c]=true;
                q.push({r,c});
            }
        }
    }
    int count=0;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<m;j++)
        {
            if(grid[i][j]==1 && vis[i][j]==false)
            {
                count++:
            }
        }
    }
    return count;
}
```
- Time Complexity : O(n*m) + O(n*m*4) + O(n*m) = O(n*m)
- Space Complexity : O(n*m) (visited matrix) + O(n*m) (queue)

### DFS
