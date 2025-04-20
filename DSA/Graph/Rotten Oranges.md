# Rotten Oranges

- You are given an m x n grid where each cell can have one of three values:
  1. 0 representing an empty cell,
  2. 1 representing a fresh orange, or
  3. 2 representing a rotten orange.
- Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.
- Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.



### BFS:
```
vector<int> dx={0,0,1,-1};
    vector<int> dy={1,-1,0,0};
    int orangesRotting(vector<vector<int>>& grid) {
        
        int m = grid.size();
        int n = grid[0].size();
        queue<pair<int,int>> q;
        int totalFresh=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]==2)
                {
                    q.push({i,j});
                }
                if(grid[i][j]==1)
                {
                    totalFresh++;
                }
            }
        }
        if(totalFresh==0)
        {
            return 0;
        }
        int time=-1;
        int count=0;
        while(!q.empty())
        {
            int size = q.size();
            time++;
            for(int j=0;j<size;j++)
            {
                auto p = q.front();
               q.pop();
            for(int i=0;i<4;i++)
            {
                int x = p.first + dx[i];
                int y = p.second + dy[i];
                if(x>=0 && x<m && y>=0 && y<n && grid[x][y]==1)
                {
                    count++;
                    grid[x][y]=2;
                    q.push({x,y});
                }
            }
            }
        }
        if(count==totalFresh)
        {
            return time;
        }
        return -1;
    }
```
- Time Complexity: O(n*m) + O(n*m*4) = O(n*m)
