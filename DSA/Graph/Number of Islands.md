# Number of Islands:
- Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Leetcode Link: https://leetcode.com/problems/number-of-islands/description/


### BFS:
```
vector<int> dx={1,-1,0,0};
    vector<int> dy={0,0,-1,1};
    void bfs(int u, int v, vector<vector<char>>& grid,  vector<vector<bool>>& vis )
    {
        queue<pair<int,int>> q;
        q.push({u,v});
        vis[u][v]=true;
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            int x = p.first;
            int y = p.second;
            for(int j=0;j<4;j++)
            {
                int r = x + dx[j];
                int c = y + dy[j];
                if(r>=0 && r<grid.size() && c>=0 && c<grid[0].size() && grid[r][c]=='1' && vis[r][c]==false)
                {
                    vis[r][c]=true;
                    q.push({r,c});
                }
            }
        }
    }
    int numIslands(vector<vector<char>>& grid) {
        
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<bool>> vis(m,vector<bool>(n,false));
        int count=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]=='1' && vis[i][j]==false)
                {
                    count++;
                    bfs(i,j,grid,vis);
                }
            }
        }
        return count;
        
    }
```
- Time Complexity: O(m*n) (each box of the matrix is processed only once)
- Space Complexity: O(m*n) (visited array) + O(min(m,n)) (max queue space)

### DFS:
```
vector<int> dx={0,0,1,-1};
    vector<int> dy={1,-1,0,0};
    void dfs(int u, int v, vector<vector<char>>& grid,  vector<vector<bool>>& vis )
    {
        vis[u][v]=true;
        for(int i=0;i<4;i++)
        {
            int x = u + dx[i];
            int y = v + dy[i];
            if(x>=0 && x<grid.size() && y>=0 && y<grid[0].size() && grid[x][y]=='1' && vis[x][y]==false)
            {
                dfs(x,y,grid,vis);
            }
        }

    }
    int numIslands(vector<vector<char>>& grid) {
        
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<bool>> vis(m,vector<bool>(n,false));
        int count=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]=='1' && vis[i][j]==false)
                {
                    count++;
                    dfs(i,j,grid,vis);
                }
            }
        }
        return count;
        
    }
```
- Time Complexity: O(m*n) (each box of the matrix is processed only once)
- Space Complexity: O(m*n) (visited array) + O(min(m,n)) (max queue space)