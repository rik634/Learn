# Bridges in Graph - Using Tarjan's Algorithm of time in and low time
- There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.
- A critical connection is a connection that, if removed, will make some servers unable to reach some other servers.
- Return all critical connections in the network in any order.

### Logic
- Bridge is the any edge on removal of whose the graph is broken down into 2 or more components.
- Here, we will take 2 arrays, "time and low". "time" contains the DFS time of insertion. The step/time at which we reach a particular node is stored in this. 
- While, "low" contains minimum lowest time of insertion of all the adjacent nodes apart from the parent.
- 

### Solution
```
class Solution {
private:
    int timer=1;
private:
    void dfs(int node, int parent, vector<bool>& vis, vector<vector<int>>& adj, vector<int>& low, vector<int>& tin,  vector<vector<int>>& bridges)
    {
        vis[node]=true;
        tin[node]=low[node]=timer;
        timer++;
        for(auto it:adj[node])
        {
            if(it==parent)
            {
                continue;
            }
            if(vis[it]==false)
            {
                dfs(it,node,vis,adj,low,tin,bridges);
                low[node]=min(low[node],low[it]);
                if(low[it]>tin[node])
                {
                    bridges.push_back({it,node});
                }
            }
            else
            {
                // it is already visited, part of the component, if we remove that edge, it is still reachable
                low[node]=min(low[node],low[it]);
            }
        }
    }
public:
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        
        // create graph from the connection
        vector<vector<int>> adj(n);
        for(int i=0;i<connections.size();i++)
        {
            // undirected graph
            adj[connections[i][0]].push_back(connections[i][1]);
            adj[connections[i][1]].push_back(connections[i][0]);
        }
        // for DFS, we will need
        vector<bool> vis(n,false);
        vector<int> tin(n,0);
        vector<int> low(n,0);
        // to store the bridges
        vector<vector<int>> bridges;
        dfs(0,-1,vis,adj,low,tin,bridges);
        return bridges;
    }
};
```
- Time Complexity: O(V+2E) (DFS traversal)
- Space Complexity: O(V+2E) + O(3*V) + 