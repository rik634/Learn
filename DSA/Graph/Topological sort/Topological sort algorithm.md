# Topological sort algorithm
- Given a DAG( Directed Acyclic Graph ), print all the vertex of the graph in a topologically sorted order. If there are multiple solutions, print any.

- Topological sort only exists for DAG (Directed Acyclic Graph)
### Logic
- Definition for topo sort: Linear ordering of vertices such that, there is an edge between u and v, then u appears before v in that ordering.

### BFS
- Implementing topological sort using BFS is also known as "Kahn's Algorithm".
- Indegree: Number of incoming edges to a node

```
vector<int> topoSort(int V, vector<vector<int>>& edges) {
        // code here
        vector<int> adj[V];
        vector<int> indegree(V,0);
        for(int i=0;i<edges.size();i++)
        {
            adj[edges[i][0]].push_back(edges[i][1]);
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
                indegree[it]--;
                if(indegree[it]==0)
                {
                    q.push(it);
                }
            }
        }
        return res;
    }
```
- Time Complexity:O(V+E)
- Space Complexity: O(V)
### DFS
```
bool dfs(int node, vector<int> adj[], vector<bool>& vis, stack<int>& s)
    {
        vis[node]=true;
        for(auto it: adj[node])
        {
            if(vis[it]==false)
            {
                dfs(it,adj,vis,s);
            }
        }
        s.push(node);
    }
    vector<int> topoSort(int V, vector<vector<int>>& edges) {
        // code here
        vector<int> adj[V];
        for(int i=0;i<edges.size();i++)
        {
            adj[edges[i][0]].push_back(edges[i][1]);
        }
        stack<int> s;
        vector<bool> vis(V,false);
        for(int i=0;i<V;i++)
        {
            if(vis[i]==false)
            {
                dfs(i,adj,vis,s);
            }
        }
        vector<int> res;
        while(!s.empty())
        {
            auto it = s.top();
            s.pop();
            res.push_back(it);
        }
        return res;
    }
```
- Time Complexity: O(V+E) (time compleixty for DFS) + O(V) (traversal through stack)
- Space Complexity: O(2*V)

### Intuition
- 