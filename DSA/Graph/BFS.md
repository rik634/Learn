# BFS (Breadth First Search) of a graph:

- It is also known as the Level Order Traversal.

```
vector<int> bfs(int V, vector<int> adj[])
{
    vector<bool> vis(n,false);
    via[0]=true;
    vector<int> res;
    queue<int> q;
    q.push(1);
    while(!q.empty())
    {
        auto p = q.front();
        q.pop();
        res.push_back(p);
        vis[p]=true;
        for(auto it: adj[p])
        {
            if(vis[it]==false)
            {
                q.push(it);
            }
        }
    }
    return res;
}

```
- Time Complexity: O(V)
- Space Complexity: O(V) + O(2E)