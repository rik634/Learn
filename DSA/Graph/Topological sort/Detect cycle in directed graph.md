# Detect cycle in directed graph
- In a directed graph, 2 nodes are connected with each other using a irected edge.


### Logic
- The same logic as used for cycle detection in undirected graph can not be applied here, due to the directed edges.
- In case of directed graph, we have to implement a logic which checks if we are revisiting any node while traversing the same path, not different path.


### DFS
```
bool dfs(int node, vector<bool>& vis, vector<bool>& pathVis, vector<int> adj[])
    {
        vis[node]=true;
        pathVis[node]=true;
        for(auto it:adj[node])
        {
            if(vis[it]==false)
            {
               if(dfs(it,vis,pathVis,adj)==true)
               {
                   return true;
               }
            }
            else if(pathVis[it]==true)
            {
                return true;
            }
        }
        pathVis[node]=false;
        return false;
    }
    bool isCyclic(int V, vector<vector<int>> &edges) {
        // code here
        vector<bool> vis(V,false);
        vector<bool> pathVis(V,false);
        vector<int> adj[V];
        for(int i=0;i<edges.size();i++)
        {
            adj[edges[i][0]].push_back(edges[i][1]);
        }
        for(int i=0;i<V;i++)
        {
            if(vis[i]==false)
            {
                if(dfs(i,vis,pathVis,adj)==true)
                {
                    return true;
                }
            }
        }
        return false;
    }
```
- Time Complexity:O(V+E) (DFS with directed edges)
- Space Complexity: O(2*V)

### BFS
- Here, we use topo sort/ Kahn's algorithm. Also, we know that topological sort is only applicable on the acyclic graph, and not applicable on cyclic graph.
- If the topo sort contains exatly N elements, the graph does not have cycle. Or else if the topo sort has any elements less than N, then it has cycle.

```
bool isCyclic(int V, vector<vector<int>> &edges) {
        
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
        return res.size()!=V;
    
    }
```

- Time Complexity:O(V+E)
- Space Complexity: O(V)

- Instead of creating an array to store the topological sort, we can use a variable "count", and based on its value, we can determine if the cycle exists or not.
