# Bellman Ford Algorithm
- Given a weighted, directed and connected graph of V vertices and E edges, Find the shortest distance of all the vertices from the source vertex S.

- Note: If the Graph contains a negative cycle then return an array consisting of only -1.

### Logic
- This algorithm helps to find the shortest path.
- This algorithm is the improvised version of Dijkstra's algorithm. Dijkstra's algorithm fails for the graph with negative edges. This is resolved in Bellman Ford algorithm.
- If the graph has the negative weight cycle, the Dijkstra's algorithm will give TLE, means it will keep running.
- Bellman ford algorithm helps to detect the negative weight cycles. And it is applicable only for the case of directed graphs. If we want to implement this algorithm in undirected graph, we need to convert the undirected graph into directed one.
- Negative weight cycles mean, path weight (sum of edge weights along the path) is negative.
- Edges can be given in any order in the vector,

- Steps for the Bellman Ford algo:
1. Relax all the edges N-1 times sequentially
   - Relaxation of edges is as follows:
    ```
    // for the directed edge from U to V, with weight W.
    if(dist[U] + W <dist[V])
    {
        dist[V] = dist[U] + W;
    }
    ```

### Solution
```
vector<int> bellmanFord(int V, vector<vector<int>>& edges, int src) {
        // Code here
        vector<int> dist(V,1e8);
        dist[src]=0;
        for(int i=0;i<V-1;i++)
        {
            for(auto it:edges)
            {
                int u = it[0];
                int v = it[1];
                int wt = it[2];
                if(dist[v]>(dist[u]+wt) && dist[u]!=1e8)
                {
                    dist[v]=dist[u]+wt;
                }
            }
        }
        for(auto it:edges)
        {
            int u = it[0];
                int v = it[1];
                int wt = it[2];
                if(dist[v]>(dist[u]+wt) && dist[u]!=1e8)
                {
                    return {-1};
                }
        }
        return dist;
        
    }
```
- Time Complexity: O(V*E) (V= Number of nodes, E = Number of edges)
- Space Complexity: O(V)


### Intuition:

1. Why N-1 times of relaxation?
- As we know that the edges can be in any order in the vector, considering the worst case scenario, where the edges are ordered in the descending order, we will need maximum of N-1 iteration to get the shortest path. So, we will be relaxing the edges N-1 times to get the shortest distance for all the nodes.

2. How to detect the negative cycles?
- In normal scenario, after the relaxation N-1 times, in the Nth iteration, dist[] value for any node will not get updated.
- If the graph has negative edge weights, on the Nth iteration, the relaxation will be done, and dist[] value get updated/reduced. This means that we are in a loop.  In such case, we can say that the graph contains the negative weight cycles.
