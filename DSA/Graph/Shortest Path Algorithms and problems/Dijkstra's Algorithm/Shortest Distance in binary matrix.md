# Shortest distance/path in binary matrix/maze - Dijkstra's Algorithm
- Given an n x n binary matrix grid, return the length of the shortest clear path in the matrix. If there is no clear path, return -1.

A clear path in a binary matrix is a path from the top-left cell (i.e., (0, 0)) to the bottom-right cell (i.e., (n - 1, n - 1)) such that:

All the visited cells of the path are 0.
All the adjacent cells of the path are 8-directionally connected (i.e., they are different and they share an edge or a corner).
The length of a clear path is the number of visited cells of this path.

### Logic


### Using priority queue
```
 vector<int> dx={1,1,0,-1,-1,-1,0,1};
    vector<int> dy={0,-1,-1,-1,0,1,1,1};
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        
        int n = grid.size();
        int m = grid[0].size();
        if(grid[0][0]==1)
        {
            return -1;
        }
        vector<vector<int>> dist(n,vector<int>(m,1e9));
        dist[0][0]=1;
        
        priority_queue<pair<int,pair<int,int>>,vector<pair<int,pair<int,int>>>, greater<pair<int,pair<int,int>>>> q;
        q.push({1,{0,0}});
        while(!q.empty())
        {
            auto p = q.top();
            q.pop();
            int distance = p.first;
            int x = p.second.first;
            int y = p.second.second;
            if(x==n-1 && y==m-1)
            {
                return distance;
            }
            for(int i=0;i<8;i++)
            {
                int r = x + dx[i];
                int c = y + dy[i];
                if(r>=0 && r<n && c>=0 && c<m && (dist[r][c]>distance+1) && grid[r][c]==0)
                {
                    dist[r][c]=distance+1;
                    q.push({dist[r][c],{r,c}});
                }
            }
        }
        return -1;
    }
```
- Time Complexity: (m*n)*(log(heap size)+8*log(heap size)) = 9*m*n*log(heap size) = m*n*log(m*n)
- If we use queue, time complexity will be reduced to : O(8*m*n)
- as we are storing the values in the queue level wise, such that the distance is in increasing order. So, even if we don't use the priority_queue and use queue, it will still be stored in ascending order. So, here, we can remove this log(m*n) extra time compleixty by using queue.
- Space Complexity: O(m*n)


### By using queue
```
 vector<int> dx={1,1,0,-1,-1,-1,0,1};
    vector<int> dy={0,-1,-1,-1,0,1,1,1};
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        
        int n = grid.size();
        int m = grid[0].size();
        if(grid[0][0]==1)
        {
            return -1;
        }
        vector<vector<int>> dist(n,vector<int>(m,1e9));
        dist[0][0]=1;
        
        queue<pair<int,pair<int,int>>> q;
        q.push({1,{0,0}});
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            int distance = p.first;
            int x = p.second.first;
            int y = p.second.second;
            if(x==n-1 && y==m-1)
            {
                return distance;
            }
            for(int i=0;i<8;i++)
            {
                int r = x + dx[i];
                int c = y + dy[i];
                if(r>=0 && r<n && c>=0 && c<m && (dist[r][c]>distance+1) && grid[r][c]==0)
                {
                    dist[r][c]=distance+1;
                    q.push({dist[r][c],{r,c}});
                }
            }
        }
        return -1;
    }
```
- Time Complexity: O(8*m*n)
- Space Complexity: O(m*n)


