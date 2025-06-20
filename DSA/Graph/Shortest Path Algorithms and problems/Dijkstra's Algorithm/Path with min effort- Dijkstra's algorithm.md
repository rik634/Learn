# Path with minimum effort
- You are a hiker preparing for an upcoming hike. You are given heights, a 2D array of size rows x columns, where heights[row][col] represents the height of cell (row, col). You are situated in the top-left cell, (0, 0), and you hope to travel to the bottom-right cell, (rows-1, columns-1) (i.e., 0-indexed). You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.

- A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

- Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

### Logic


### Using priority queue
```
vector<int> dx={0,0,1,-1};
    vector<int> dy={1,-1,0,0};
    int minimumEffortPath(vector<vector<int>>& heights) {
        
        int n = heights.size();
        int m = heights[0].size();
        vector<vector<int>> dist(n,vector<int>(m,1e9));
        dist[0][0]=0;
        
        priority_queue<pair<int,pair<int,int>>,vector<pair<int,pair<int,int>>>,greater<pair<int,pair<int,int>>>> q;
        q.push({0,{0,0}});
        while(!q.empty())
        {
            auto p = q.top();
            q.pop();
            int effort = p.first;
            int x = p.second.first;
            int y = p.second.second;
            if(x==n-1 && y ==m-1)
            {
                return effort;
            }
            for(int i=0;i<4;i++)
            {
                int r = x + dx[i];
                int c = y + dy[i];
                if(r>=0 && r<n && c>=0 && c<m && dist[r][c]>(max(effort,abs(heights[r][c]-heights[x][y]))) )
                {
                    dist[r][c] = max(effort,abs(heights[r][c]-heights[x][y]));
                    q.push({dist[r][c],{r,c}});
                }
            }
        }
        return -1;
    }
```
- Time Complexity: E*log(V) (time complexity for Dijkstra's algorithm) (in case of matrix, E = 4*m*n, V = m*n)= 4*m*n*log(m*n)
- Space Complexity: O(n*m)
