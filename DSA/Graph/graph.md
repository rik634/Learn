# Graph

- If we have a structure which had nodes, edges etc, it is known as graph.

- Types: Directed graph, Undirected Graph.
- Node/ Vertex
- Edge
- Cycle in a graph: Start at a node, and end up at the same node.
- DAG: Directed Acyclic Graph
- Path: Contains a lot og nodes and edges, each of them are reachable. Here, we ca not have the same node appearing 2 times.


### Degree of Graph:

- For undirected graph:
  1. degree of a node is the number of edges attached to it.
  2. Total degree of graph = Sum of the degree of all the nodes = 2 * Number of edges
- For directed graph:
  1. Indegree of a node is equal to the number of edges going inside of the node
  2. Outdegree of a node is equal to the number of edges going outside of the node. 

## Graph Representation:

- N = Number of nodes
- M = Number of edges
 
### Adjacency Matrix:
- If there is an edge between i and j:
  1. For unweighted graph, we will store adj[i][j]=1;
  2. For weighted graph: adj[i][j]=weight
```
int main()
{
    int n,m;
    cin >> n >> m;
    vector<vector<int>> adj(n+1,vector<int>(n+1,0));
    for(int i=0;i<m;i++)
    {
        cin >> u >> v;
        adj[u][v]=1;
        adj[v][u]=1; // Only for the undirected graph
    }
}
```
- adj[i][j]: This represents if there is an edge between the node i and j.
- Space Complexity: O(n*m)

### Adjacency List:
- For unweighted graph, we will just store the edges
```
vector<int> adj[n+1];
```
- For the weighted graph, we will store the weights along with the another node.
```
vector<pair<int,int>> adj[n+1];
```
```
int main()
{
    int n,m;
    cin >> n >> m;
    vector<int> adj[n+1];
    for(int i=0;i<m;i++)
    {
        int u,v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u); //Only for undirected graph
    }
}
```
- Space Complexity: O(2*m)

