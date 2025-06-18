# Bipartite Graph
- There is an undirected graph with n nodes, where each node is numbered between 0 and n - 1. You are given a 2D array graph, where graph[u] is an array of nodes that node u is adjacent to. More formally, for each v in graph[u], there is an undirected edge between node u and node v. The graph has the following properties:

1. There are no self-edges (graph[u] does not contain u).
2. There are no parallel edges (graph[u] does not contain duplicate values).
3. If v is in graph[u], then u is in graph[v] (the graph is undirected).
4. The graph may not be connected, meaning there may be two nodes u and v such that there is no path between them.
5. A graph is bipartite if the nodes can be partitioned into two independent sets A and B such that every edge in the graph connects a node in set A and a node in set B.

- Return true if and only if it is bipartite.

### Logic
- Basically, it is asking if we can color the graph with 2 colors such that no adjacent nodes have the same color.
- Linear graph with no cycle, is always bipartite
- A graph with even cycle length is also bipartite.
- A graph with odd cycle length is always bipartite.

- Here, we are assuming that there can be more than one components present in the graph.
### BFS
```
bool bfs(int source, vector<vector<int>>& graph, vector<int>& color)
    {
        queue<int> q;
        q.push(source);
        color[source]=0;
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
           
            for(auto it:graph[p])
            {
                if(color[it]==-1)//not yet colored
                {
                    color[it]=!color[p];
                    q.push(it);
                }
                else if(color[it]==color[p])
                {
                    return false;
                }

            }
        }
        return true;
    }
    bool isBipartite(vector<vector<int>>& graph) {
        
        int n = graph.size();//number of nodes
        
        vector<int> color(n,-1);
        for(int i=0;i<n;i++)
        {
            if(color[i]==-1)
            {
                if(bfs(i,graph,color)==false)
                {
                    return false;
                }
            }
        }
        return true;
    }
```
- Time Complexity: O(V+2*E) ( same as the time complexity for BFS)
- Space Complexity: O(V) (color array)

### DFS
```
bool dfs(int node,int colour, vector<vector<int>>& graph, vector<int>& color)
    {
        color[node]=colour;
        int newColor = !colour;
        for(auto it:graph[node])
        {
            if(color[it]==-1)
            {
                if(dfs(it,newColor,graph,color)==false)
                {
                    return false;
                }
            }else if(color[it]==colour)
            {
                return false;
            }
        }
        return true;
    }
    bool isBipartite(vector<vector<int>>& graph) {
        
        int n = graph.size();//number of nodes
        
        vector<int> color(n,-1);
        for(int i=0;i<n;i++)
        {
            if(color[i]==-1)
            {
                if(dfs(i,0,graph,color)==false)
                {
                    return false;
                }
            }
        }
        return true;
    }
```
- Time Complexity: O(V+2*E) (default DFs algo time complexity)
- Space Complexity: O(V)
