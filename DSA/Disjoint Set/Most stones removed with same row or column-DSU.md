# Most stones removed with same row or column
- There are n stones at some integer coordinate points on a 2D plane. Each coordinate point may have at most one stone.
- You need to remove some stones. 
- A stone can be removed if it shares either the same row or the same column as another stone that has not been removed.
- Given an array of stones of length n where stones[i] = [xi, yi] represents the location of the ith stone, return the maximum possible number of stones that you can remove.

### Logic
- Here, if we see, we can connect the stones based on the condition that they are either present in same row or same column.
- For each component containing n stones, we can remove n-1 from them, rest 1 will stay as it is.
- So, total stones that can be removed are: totalStones - No. of components
- When we hear about connected components, we for traversal algorithm using BFS or DFS.
- But, here, we are connecting the stones present at different positions. And in connections, we don't use the traversals. We prefer to use the disjoint set.
- When using the disjoint set for the 2d matrix, we have to convert the indices (i,j) into a single integer. So, here, we will treat a row as a node. Same in case of columns.
- If there is a stone ata row-0, at any column, we can say that it belongs to node 0.
- As for columns, we have n rows, the column-0, will be numbered from n+1. And column-0 will be considered as the node-n+1.
- If there is a stone present at (i,j), we will combine(union) of ith node, and (numberOfColumns + j + 1)
- Here, we just need the nodes in Disjoint set which are involved in having a stone. So, we store the rows and columns as they will have stones. And we just need to count them once for ultimate parents.

### Solution
```
class DisjointSet{
  
public:
vector<int> rank, parent, size;
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
    int removeStones(vector<vector<int>>& stones) {
        
        int n = stones.size(); // number of stones
        int maxRowIndex =0;
        int maxColIndex =0;
        for(auto it:stones)
        {
            maxRowIndex = max(maxRowIndex, it[0]);
            maxColIndex = max(maxColIndex,it[1]);
        }
        DisjointSet ds(maxRowIndex+maxColIndex+1); // +1 for safe-side
        unordered_map<int,int> stoneNodes;
        for(auto it:stones)
        {
            int row = it[0];
            int col = it[1] + maxRowIndex + 1;
            ds.unionBySize(row, col);
            stoneNodes[row]=1;
            stoneNodes[col]=1;
        }
        int count=0;
        for(auto it:stoneNodes)
        {
            if(ds.findUPar(it.first)==it.first)
            {
                count++;
            }
        }
        return n-count; 

    }
};
```
- Time Complexity: O(N) + O(N) + O(4*alpha) = O(N)
- Space Complexity: O(2*(maxRowIndex+maxColIndex)) (parent and size array)