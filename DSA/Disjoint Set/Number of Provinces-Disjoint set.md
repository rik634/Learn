# Number of Provinces - Disjoint set
- Given an undirected graph with V vertices. We say two vertices u and v belong to a single province if there is a path from u to v or v to u. Your task is to find the number of provinces.

- Note: A province is a group of directly or indirectly connected cities and no other cities outside the group.

### Logic
- The number of components/provinces, number of unique parents in parent array, is equal to the number of times "parent[i]==i".

### Solution
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
};
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        
        int V = isConnected.size();
        DisjointSet ds(V);
        for(int i=0;i<V;i++)
        {
            for(int j=0;j<V;j++)
            {
                if(isConnected[i][j]==1)
                {
                    ds.unionBySize(i,j);
                }
            }
        }
        int count=0;
        for(int i=0;i<V;i++)
        {
            if(ds.findUPar(i)==i)
            {
                count++;
            }
        }
        return count;
    }
};
```
- Time Complexity: O(N^2) + O(N*4*alpha) (let's say, each node is independent, and for each of those, we are calling the findUPar function )
- Space Complexity: O(N^2)
