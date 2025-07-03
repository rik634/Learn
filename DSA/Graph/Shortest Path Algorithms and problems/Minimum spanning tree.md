# Minimum spanning tree
- Given a weighted, undirected, and connected graph of V vertices and E edges. The task is to find the sum of weights of the edges of the Minimum Spanning Tree.
- A spanning tree is the tree in which there are N nodes, and N-1 edges and all the nodes are reachable from each other.
- A minimum spanning tree is the spanning tree with minimum cost (sun of edge weights).
- There are 2 algorithms used to get the minimum spanning tree:
  1. Prism's Algorithm
  2. Kruskal's Algorithm

### Logic


### Using Prim's Algorithm
```
int spanningTree(int V, vector<vector<int>> adj[]) {
        // code here
        
        priority_queue<pair<int,int>,vector<pair<int,int>>, greater<pair<int,int>>> q;
        vector<bool> vis(V,false);
        q.push({0,0});
        
        int sum=0;
        while(!q.empty())
        {
            auto p = q.top();
            q.pop();
            int node = p.second;
            int wt = p.first;
            if(vis[node]==true)
            {
                continue;
            }
            //adding to mst
            vis[node]=true;
            sum+=wt;
            for(auto it:adj[node])
            {
                if(vis[it[0]]==false)
                {
                    q.push({it[1],it[0]});
                }
            }
        }
        return sum;
    }
```
- Time Complexity: E*logE + E*logE = E*logE
- Space Complexity: O(E+V) (E= number of edges, due to priority queue, V = Number of nodes, due to visited array)

### Intuition:
- Greedy is the intuition of this algorithm.
- Thw while loop is running E times. For the top ad push operation in in min heap, time complexity will be log(max elements in queue). In the worst case, the max size of queue will be E, i.e., all the edges will be in the queue.
- for adjacency list traversal, it can be max E times, inside loop operation contain push, i.e., logE
- Time complexity = E *(logE) + E*(logE) = E*logE

### Using Kruskal's Algorithm
- Here, we will make use of disjoint set
```
// User function Template for C++
class DisjointSet{
  vector<int> rank, parent, size;
public:
  DisjointSet(int n)
  {
    rank.resize(n+1,0);
    size.resize(n+1,1);
    parent.resize(n+1);
    for(int i=0;i<=n;i++)
    {
      parent[i]=i;
    }
  }
  int findUPar(int node)
  {
    if(node==parent[node])
    {
      return node;
    }
    return parent[node]=findUPar(parent[node]);
  }
  void unionByRank(int u, int v)
  {
    int pu = findUPar(u);
    int pv = findUPar(v);
    if(pu==pv)
    {
      return;
    }
    if(rank[pu]<rank[pv])
    {
      parent[pu]=pv;
    }
    else if(rank[pv]<rank[pu])
    {
      parent[pv]=pu;
    }
    else
    {
      parent[pv]=pu;
      rank[pu]++;
    }
  }
  void unionBySize(int u, int v)
  {
    int pu = findUPar(u);
    int pv = findUPar(v);
    if(pu==pv)
    {
      return;
    }
    if(size[pu]<size[pv])
    {
      parent[pu]=pv;
      size[pv]+=size[pu];
    }
    else
    {
      parent[pv]=pu;
      size[pu]+=pv;
    }
  }
};
class Solution {
  public:
    int kruskalsMST(int V, vector<vector<int>> &edges) {
        // code here
        vector<pair<int,pair<int,int>>> v;
        for(auto it:edges)
        {
            v.push_back({it[2],{it[0],it[1]}});
            v.push_back({it[2],{it[1],it[0]}});
        }
        int weight=0;
        DisjointSet ds(V);
        sort(v.begin(),v.end());
        for(auto it:v)
        {
            if(ds.findUPar(it.second.first)!=ds.findUPar(it.second.second))
            {
                weight+=it.first;
                ds.unionBySize(it.second.first,it.second.second);
            }
        }
        return weight;
    }
};
```
- Time Complexity:  O(V+E) + O(E*logE) (sorting) + O(E*4*alpha*2)
- Space Complexity: O(V*3) (size, rank, parent) + O(V) (v)
