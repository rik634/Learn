# Shortest path in undirected graph with unit distance
- Given an Undirected Graph having unit weight, find the shortest path from the source to all other nodes in this graph. In this problem statement, we have assumed the source vertex to be ‘0’. If a vertex is unreachable from the source node, then return -1 for that vertex.

### Logic


### BFS
```
vector<int> shortestPath(vector<vector<int>>& adj, int src) {
        // code here
        int V = adj.size();
        vector<int> dist(V,1e9);
        dist[src]=0;
        queue<int> q;
        q.push(src);
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            int d = dist[p];
            for(auto it:adj[p])
            {
                if((dist[p]+1)<dist[it])
                {
                    dist[it]=1+dist[p];
                    q.push(it);
                }
            }
        }
        for(int i=0;i<V;i++)
        {
            if(dist[i]==1e9)
            {
                dist[i]=-1;
            }
        }
        return dist;
    }
```
- Time Complexity:O(V+2*E) (BFS time complexity for undirected graph) + O(V)
- Space Complexity:O(V) (dist) + O(V) (queue)


