# Flood Fill Algorithm

### BFS:
```
 vector<int> dx={0,0,1,-1};
    vector<int> dy= {1,-1,0,0};
    void bfs(int u, int v, int color, vector<vector<int>>& image )
    {
        image[u][v]=color;
        queue<pair<int,int>> q;
        q.push({u,v});
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            for(int i=0;i<4;i++)
            {
                int a = p.first + dx[i];
                int b = p.second + dy[i];
                if(a>=0 && a<image.size() && b>=0 && b<image[0].size() && image[a][b]==-1)
                {
                    image[a][b]=color;
                    q.push({a,b});
                }
            }
        }
    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        
        int c = image[sr][sc];
        int m = image.size();
        int n = image[0].size();
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(image[i][j]==c)
                {
                    image[i][j]=-1;
                }
            }
        }
        bfs(sr,sc,color,image);
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(image[i][j]==-1)
                {
                    image[i][j]=c;
                }
            }
        }
        return image;
    }
```
- Time Complexity: O(n*m)
- Space Complexity: O(n*m)

### DFS:
```
 vector<int> dx={0,0,1,-1};
    vector<int> dy= {1,-1,0,0};
    void dfs(int u, int v, int color, vector<vector<int>>& image )
    {
        image[u][v]=color;
        for(int i=0;i<4;i++)
        {
            int x = u + dx[i];
            int y = v + dy[i];
            if(x>=0 && x<image.size() && y>=0 && y<image[0].size() && image[x][y]==-1 )
            {
                dfs(x,y,color,image);
            }
        }
    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        
        int c = image[sr][sc];
        int m = image.size();
        int n = image[0].size();
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(image[i][j]==c)
                {
                    image[i][j]=-1;
                }
            }
        }
        dfs(sr,sc,color,image);
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(image[i][j]==-1)
                {
                    image[i][j]=c;
                }
            }
        }
        return image;
    }
```
- Time Complexity: O(n*m)
- Space Complexity: O(n*m) + O(n*m) (recursive stack space)
