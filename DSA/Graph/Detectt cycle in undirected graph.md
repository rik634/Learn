# Detect cycle in undirected graph using BFS:

- Cycle in a graph: Starts at one node and ends at the same node.


### Logic:
- The graph can be divided into multiple components. So, we will take each of the nodes, which is not visited yet, as source and detect the cycle for it.

### Using BFS:
```
bool detect(int src, vector<int> adj[], vector<bool>& vis)
{
    vis[src]=true;
    queue<pair<int,int>> q;
    q.push({src,-1});
    while(!q.empty())
    {
        auto p = q.front();
        q.pop();
        int node = p.first;
        int parent = p.second;
        for(auto it:adj[node])
        {
            if(vis[it]==false)
            {
                vis[it]=true;
                q.push({it,node});
            }
            else if(parent!=it)
            {
                return true;
            }
        }
    }
}
bool isCycle(int V, vector<int> adj[])
{
    vector<bool> vis(V+1,false);
    for(int i=1;i<=V;i++)
    {
        if(vis[i]==false)
        {
            if(detect(i,adj,vis)==true)
            {
                return true;
            }
        }
    }
    return false;
}
```
- Time Complexity: O(V+2*E) (For each of the nodes, we are also visiting the edges nodes for that, and as it is undirected graph, it will be 2 times) + O(V) (in main function) = O(V+2*E)
- Space Complexity: O(V)

### Using DFS:
- This code is i=useful when there is only one component formed from the graph nodes
```
bool dfs(int node, int parent, vector<bool>& vis, vector<int> adj[])
{
    vis[node]=true;
    for(auto it:adj[node])
    {
        if(vis[it]==false)
        {
            if(dfs(it,node,vis,adj)==true)
            {
                return true;
            }
        }
        else if(it!=parent)
            {
                return true;
            }
    }
    return false;

}

bool isCycle(int V, vector<int> adj[])
{
    vector<bool> vis(V+1,false);
    return dfs(1,-1, vis, adj);
}

```
- Time Complexity: O(V+2E) (For DFS);
- Space Complexity: O(V) (Recursion stack space) + O(V) (visited array) = O(N)

- For multi-component graph:
```
bool dfs(int node, int parent, vector<bool>& vis, vector<int> adj[])
{
    vis[node]=true;
    for(auto it:adj[node])
    {
        if(vis[it]==false)
        {
            if(dfs(it,node,vis,adj)==true)
            {
                return true;
            }
        }
        else if(it!=parent)
            {
                return true;
            }
    }
    return false;

}

bool isCycle(int V, vector<int> adj[])
{
    vector<bool> vis(V+1,false);
    for(int i=1;i<=V;i++)
    {
        if(vis[i]==false)
        {
            if(dfs(i,-1,vis,adj)==true)
            {
                return true;
            }
        }
    }
    return false;
}
```
- Time Complexity: O(V+2E) (For DFS) + O(N) (for loop for multi component condition)
- Space Complexity: O(V) (Recursion stack space) + O(V) (visited array) = O(N)
