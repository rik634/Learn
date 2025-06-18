# Number of Distinct Islands
- Given a boolean 2D matrix grid of size N x M. You have to find the number of distinct islands where a group of connected 1s (horizontally or vertically) forms an island. Two islands are considered to be distinct if and only if one island is equal to another (not rotated or reflected).

### Logic
- If the islands(group od 1's) are at different positions, but have same shape, we call it as identical.
- Here, we will save the shapes of the island in set, and set will give us the number of distinct islands
- Here, in order to store the shape, we will consider one of the cells as the base. And to get the value for rest, we will substarctt base from it.
- Example: for (2,3) when base of (1,0), ans will be (2-1,3-0) = (1,3)
- In order to make sure that we follow same pattern while traversing any particular island, we perform DFS.

### DFS
```
 //O(1) space complexity
 vector<int> dx={1,0,-1,0};
    vector<int> dy={0,-1,0,1};
    void dfs(int u, int v, vector<vector<bool>>& vis, vector<vector<int>>& grid, vector<pair<int,int>>& vec, int sr, int sc)
    {
        vis[u][v]=true;
        if(sr==-1 && sc==-1)
        {
            sr=u;
            sc=v;
        }
        vec.push_back({u-sr,v-sc});
        for(int l=0;l<4;l++)
        {
            int r = u + dx[l];
            int c = v + dy[l];
            if(r< grid.size() && r>=0 && c>=0 && c<grid[0].size() && vis[r][c]==false && grid[r][c]==1)
            {
                dfs(r,c,vis,grid,vec,sr,sc);
            }
        }
        
    }
    int countDistinctIslands(vector<vector<int>>& grid) {
        // code here
        int n = grid.size();
        int m = grid[0].size();
        //O(n*m) space complexity
        vector<vector<bool>> vis(n,vector<bool>(m,false));
        //O(n*m) space complexity
        set<vector<pair<int,int>>> s;
        //O(n*m) time complexity
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(grid[i][j]==1 && vis[i][j]==false)
                {
                    vector<pair<int,int>> vec;
                    dfs(i,j,vis,grid,vec, -1,-1);
                    s.insert(vec);
                }
            }
        }
        return s.size();
        
    }
```
- Time Complexity:O(n*m) + O(n*m*4) (DFS traversal)
- Space Complexity: O(n*m*log(set-length, which is n*m)) (visited matrix) + O(n*m) (set) + O(position vectors) 

