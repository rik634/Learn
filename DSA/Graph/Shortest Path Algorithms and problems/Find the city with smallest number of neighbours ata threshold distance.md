# Find the city with the smallest number of neighbors at a threshold distance
-  There are n cities numbered from 0 to n-1. Given the array edges where edges[i] = [from_i, toi,weight_i]  represents a bidirectional and weighted edge between cities from_i and to_i, and given the integer distance Threshold. You need to find out a city with the smallest number of cities that are reachable through some path and whose distance is at most Threshold Distance, If there are multiple such cities, our answer will be the city with the greatest number.

- Note: that the distance of a path, connecting cities i and j are equal to the sum of the edges' weights along that path

### Logic
- As we need the shortest distance between all pair of nodes, we use Floyd Warshall Algorithm.

### Solution
```
 int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        
        vector<vector<int>> dist(n,vector<int>(n,1e9));
        for(int i=0;i<edges.size();i++)
        {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            dist[u][v]=w;
            dist[v][u]=w;
        }
        for(int i=0;i<n;i++)
        {
            dist[i][i]=0;
        }
        for(int k=0;k<n;k++)
        {
            for(int i=0;i<n;i++)
            {
                for(int j=0;j<n;j++)
                {
                    dist[i][j]=min(dist[i][j],dist[i][k]+dist[k][j]);
                }
            }
        }
        int mx=n;
        int city=-1;
        for(int i=0;i<n;i++)
        {
            int count=0;
            for(int j=0;j<n;j++)
            {
                if(dist[i][j]<=distanceThreshold)
                {
                    count++;
                }
            }
            if(count<=mx)
            {
                mx=count;
                city = i;
            }
            
        }
        return city;
    }
```
- Time Complexity: O(E) + O(n*n*n) + O(n*n)
- Space Complexity: O(n*n)