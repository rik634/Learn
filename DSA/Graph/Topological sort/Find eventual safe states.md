# Find eventual safe states
- A directed graph of V vertices and E edges is given in the form of an adjacency list adj. Each node of the graph is labeled with a distinct integer in the range 0 to V - 1. A node is a terminal node if there are no outgoing edges. A node is a safe node if every possible path starting from that node leads to a terminal node. You have to return an array containing all the safe nodes of the graph. The answer should be sorted in ascending order.

### Logic
- Any node which is part of a cycle, is not a safe node.
- Any node which leads to a cycle, is not a safe node.
- So, the nodes not involves in cycle and does not lead to cycle are safe nodes.
- We can go with the algorithm to detect the cycle in an directed graph.
- Here, if we complete the dfs for any particular node, it means, cycle is not present in that path, and we can mark it as safe node at end of dfs call.
- Rest of the nodes will not be the safe nodes.

### DFS
```
 bool dfs(int node, vector<vector<int>>& graph, vector<bool>& vis, vector<bool>& pathVis, vector<bool>& check)
    {
        vis[node]=true;
        pathVis[node]=true;
        for(auto it:graph[node])
        {
            if(vis[it]==false)
            {
                if(dfs(it,graph,vis,pathVis, check)==true)
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
        check[node]=true;
        return false;
    }
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        
        int n = graph.size();
        vector<bool> vis(n,false);
        vector<bool> pathVis(n,false);
        vector<bool> check(n,false);
        for(int i=0;i<n;i++)
        {
            if(vis[i]==false)
            {
                dfs(i,graph,vis,pathVis, check);
            }
        }
        vector<int> res;
        for(int i=0;i<n;i++)
        {
            if(check[i]==true)
            {
                res.push_back(i);
            }
        }
        return res;
    }
```
- Time Complexity: O(n+E) (DFS in directed graph) + O(n)
- Space Complexity: O(3*n)

### BFS
- Using topological sorting (BFS)
```
vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        
        int n = graph.size();
        vector<int> adj[n];
        vector<int> indegree(n,0);
        for(int i=0;i<n;i++)
        {
           for(auto it:graph[i])
           {
            adj[it].push_back(i);
            indegree[i]++;
           }
        }
        queue<int> q;
        vector<int> res;
        for(int i=0;i<n;i++)
        {
            if(indegree[i]==0)
            {
                q.push(i);
            }
        }
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            res.push_back(p);
            for(auto it:adj[p])
            {
                indegree[it]--;
                if(indegree[it]==0)
                {
                    q.push(it);
                }
            }
        }
        sort(res.begin(),res.end());
        return res;
    }
```
- Time Complexity:O(V+E) (reversing the edges) +  O(V+E) + O(nlogn) (for sorting in worst case)
- Space Complexity: O(V+E) + O(V)