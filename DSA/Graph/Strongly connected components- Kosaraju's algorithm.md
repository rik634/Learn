# Strongly Connected Components
- Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, Find the number of strongly connected components in the graph.

- For this logic, below questions can be asked:
  1. Number of strongly connected components
  2. Print the strongly connected components

### Logic
- Strongly connected components are valid only for directed graphs.
- In any strongly connected component, every node is reachable from every one of the nodes in that component.
- If we use DFS, it will just traverse all the nodes, and we will not be able to differentiate between component and strongly connected component.
- And the SCC's are reachable to each other, that's why, we end up traversing all the nodes when used DFS.
- If we reverse the edges, the nodes in any SCC will be reachable to all the nodes in that SCC, while any of the SCC's will not be reachable to other SCC's. 

- Steps:
  1. Sort all the edges according to the finishing time.
  2. Reverse the edges
  3. Perform DFS traversal.

- Here, we sort this, in order to make sure that, the DFS from the starting node, does not lead to traversal of all the nodes. It makes sure that 0, is not present in the first part of reversed edge graph.

### Solution
```
//Position this line where user code will be pasted.
class Solution {
  public:
    void dfs(int node, vector<vector<int>>& adj, vector<bool>& vis, stack<int>& st)
    {
        vis[node]=true;
        for(auto it:adj[node])
        {
            if(vis[it]==false)
            {
                dfs(it,adj,vis,st);
            }
        }
        st.push(node);
    }
    void dfs1(int node, vector<vector<int>>& adjT, vector<bool>& vis)
    {
        vis[node]=true;
        for(auto it:adjT[node])
        {
            if(vis[it]==false)
            {
                dfs1(it,adjT,vis);
            }
        }
       
    }
    int kosaraju(vector<vector<int>> &adj) {
        // code here
        int n = adj.size();
        //O(V)
        vector<bool> vis(n,false);
        //O(V)
        stack<int> st;
        
        // O(V+E)
        for(int i=0;i<n;i++)
        {
            if(vis[i]==false)
            {
                dfs(i,adj,vis,st);
            }
        }
        // reverse the edges
        //O(V+E)
        vector<vector<int>> adjT(n);
        for(int i=0;i<n;i++)
        {
            vis[i]=false;
            for(auto it:adj[i])
            {
                adjT[it].push_back(i);
            }
        }
        
        int count=0;
        //O(V+E)
        while(!st.empty())
        {
            int p = st.top();
            st.pop();
            if(vis[p]==false){
                count++;
                dfs1(p,adjT, vis);
            }
        }
        return count;
    }
};
```
- Time Complexity: O(3*(V+E))
- Space Complexity: O(V) + O(V) + O(V+E)