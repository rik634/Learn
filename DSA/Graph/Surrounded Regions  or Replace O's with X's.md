# Surrounded Regions | Replace O's with X's
- Given a matrix mat of size N x M where every element is either ‘O’ or ‘X’. Replace all ‘O’ with ‘X’ that is surrounded by ‘X’. An ‘O’ (or a set of ‘O’) is considered to be surrounded by ‘X’ if there are ‘X’ at locations just below, just above just left, and just right of it.

### Logic
- a set of O's that are connected to the boundary, and the O's connected to such O's, can not be converted into X's.
- Traverse around the boundary of the matric, and check of O's. For such O's, perform the DFS traversal. For the all the O's in the traversal, we will mark it as not applicable. 
- For rest of the remaining O's in the matrix, wi


### DFS
```
vector<int> dx={0,0,1,-1};
vector<int> dy = {1,-1,0,0};
void dfs(int u, int v, vector<vector<bool>>& vis, vector<vector<char>>& grid)
{
    vis[u][v]=true;
    for(int i=0;i<4;i++)
    {
        int r = dx[i] + u;
        int c = dy[i]+v;
        if(r>=0 && r<grid.size() && c>=0 && c<grid[0].size() && grid[r][c]=='O' && vis[r][c]==false)
        {
            dfs(r,c,vis,grid);
        }
    }
}
int main()
{
    vector<vector<char>> grid;
    vector<vector<bool>> vis(n,vector<bool>(m,false));
    for(int j=0;j<m;j++)
    {
        if(grid[0][j]=='O' && vis[0][j]==false)
        {
            dfs(0,j,vis,grid);
        }
        if(grid[n-1][j]=='O' && vis[n-1][j]==false)
        {
            dfs(n-1,j,vis,grid);
        } 
    }
    for(int i=0;i<n;i++)
    {
        if(grid[i][0]=='O' && vis[i][0]==false)
        {
            dfs(i,0,vis,grid);
        }
        if(grid[i][m-1]=='O' && vis[i][m-1]==false)
        {
            dfs(i,m-1,vis,grid);
        }
    }
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<m;j++)
        {
            if(vis[i][j]==false && grid[i][j]=='O')
            {
                grid[i][j]='X';
            }
        }
    }
    cout << grid;

}

```
- Time Complexity: O(M)+ O(N) + O(N*M*4) (DFS call is being made these number of times in worst condition, for 4 direction, multiplied by 4)
- Space Complexity: O(N*M) (visited array) + O(4) + O(N*M) (recursion stack space)
