# Number of Provinces
- Find the number of connected components

- There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

Leetcode Link: https://leetcode.com/problems/number-of-provinces/description/

### DFS:
```
void dfs(int node, vector<int> adj[], vector<bool>& vis)
    {
        vis[node]=true;
        for(auto it:adj[node])
        {
            if(vis[it]==false)
            {
                dfs(it,adj,vis);
            }
        }
    }
    int findCircleNum(vector<vector<int>>& isConnected) {
        // convert to adjacency list
        int n = isConnected.size();
        vector<int> adj[n];
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(isConnected[i][j]==1 && i!=j)
                {
                    adj[i].push_back(j);
                }
            }
        }
        vector<bool> vis(n,false);
        int count=0;
        for(int i=0;i<n;i++)
        {
            if(vis[i]==false)
            {
                count++;
                dfs(i,adj,vis);
            }
        }
        return count;
    }
```
- Time Complexity: O(N+2*E) (DFS)+ O(N) (loop to call DFS) + O(N^2) (matrix to list conversion) 
- Space Complexity: O(N+2*E) (adjacenu list) + O(N) (visited array) + O(N) (auxiliary stack space)