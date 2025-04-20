# Depth First Search (DFS):

- DFS is the recursive algorithm.

```
int dfs(int node, vector<int>& adj[], vector<bool>& vis, vector<int>& res)
{
    vis[node]=true;
    res.push_back(node);
    for(auto it:adj[node])
    {
        if(vis[it]==false)
        {
            dfs(it,adj,vis,res);
        }
    }
}
vector<int> dfsOfGraph(int V, vector<int> adj[])
{
    vector<bool> vis(V,false);
    int start =0;
    vector<int> res;
    dfs(start, adj, vis, res);
}
```
- Time Complexity: O(V)+O(2*E) (undirected graph), O(V+E) (directed graph)
- Space Complexity: O(V)(visited array)+O(V)(recursive stack space) + O(V) (result array)