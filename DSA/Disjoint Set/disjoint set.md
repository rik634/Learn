# Disjoint set
- It is mostly used with dynamic graph. Dynamic graphs are the graphs which keep on changing the configuration at every moment. 
- It performs 2 functions:
  1. FindParent()
  2. Union:
     - This function helps to add the edge between 2 nodes.
     - It can be done by 2 methods
       a. By rank
       b. By size

### Union by rank:
- Initially, every node is the parent of itself.
- Steps:union(u,v)
  1. Find the ultimate parent of u and v.(parents are pu and pv)
  2. Find the rank of ultimate parents, viz., pu and pv
  3. Connect smaller rank to larger rank always.

```
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
      size[pu]+=size[pv];
    }
  }
}
```
- Time complexity is same for both the methods, union by size and union by rank
- Time Complexity: O(4*alpha) ~ O(constant) (as value of alpha is close to 1)

### Intuition
1. Why connect smaller to larger?
   - If we connect larger to smaller, the height increases, and to find the parent for a particular node, we will require more time complexity. 
   - So, we prefer to connect the smaller to larger.

2. 