# Shortest Path in DAG
- Given a DAG, find the shortest path from the source to all other nodes in this DAG. In this problem statement, we have assumed the source vertex to be ‘0’. You will be given the weighted edges of the graph.


### Logic
- Here, we will be storing adjacency list as pairs, along wih the weight of edges.
- Here, first we will get the topo sort of the nodes, then we will relax the edges to get the shortest path length.

### BFS
```
 vector<int> shortestPath(int V, int E, vector<vector<int>>& edges) {
        // code here
        vector<pair<int,int>> adj[V];
        vector<int> indegree(V,0);
        for(int i=0;i<edges.size();i++)
        {
            adj[edges[i][0]].push_back({edges[i][1],edges[i][2]});
            indegree[edges[i][1]]++;
        }
        queue<int> q;
        for(int i=0;i<V;i++)
        {
            if(indegree[i]==0)
            {
                q.push(i);
            }
        }
        vector<int> res;
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            res.push_back(p);
            for(auto it: adj[p])
            {
                indegree[it.first]--;
                if(indegree[it.first]==0)
                {
                    q.push(it.first);
                }
            }
        }
        vector<int> dist(V,INT_MAX);
        dist[0]=0;
        for(int i=0;i<V;i++)
        {
            int node = res[i];
            if(dist[node]==INT_MAX)
            {
                continue;
            }
            for(auto it: adj[node])
            {
                int noden = it.first;
                int wt = it.second;
                
                if(dist[noden]>(dist[node]+wt))
                {
                    dist[noden] = dist[node]+wt;
                }
            }
        }
        for(int i=0;i<V;i++)
        {
            if(dist[i]==INT_MAX)
            {
                dist[i]=-1;
            }
        }
        return dist;
    }
```
- Time Complexity: O(V+E) (for DFS/ topo sort) + O(V+E) (for relaxing the edges)
- Space Complexity:O(V+E) + O(3*V)

### DFS
```
void topoSort(int node, vector<pair<int,int>> adj[], vector<bool>& vis, stack<int>& st) {
      
            vis[node] = 1;
            for (auto it: adj[node]) {
            int v = it.first;
            if (!vis[v]) {
                topoSort(v, adj, vis, st);
            }
        }
        st.push(node);
    }
    vector<int> shortestPath(int V, int E, vector<vector<int>>& edges) {
        // code here
       vector<pair<int,int>> adj[V];
       for (int i = 0; i < E; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int wt = edges[i][2];
            adj[u].push_back({v, wt}); 
       }
        vector<bool> vis(V,false);
        stack<int> st;
        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                 topoSort(i, adj, vis, st);
            }
        }
        vector<int> dist(V,1e9);
        dist[0] = 0;
        while (!st.empty()) {
            int node = st.top();
            st.pop();

            for (auto it: adj[node]) {
                int v = it.first;
                int wt = it.second;

                if (dist[node] + wt < dist[v]) {
                    dist[v] = wt + dist[node];
                }
            }
        }

        for (int i = 0; i < V; i++) {
            if (dist[i] == 1e9) dist[i] = -1;
        }
        return dist;
    }
```
- Time Complexity: O(V+E) (for topo sort) + O(V+E) (for relaxation)
- Space Complexity: O(V) (stack) + O(V) (dist array) + O(V) (vis vector) + O(V+2*E) (adjacency list)