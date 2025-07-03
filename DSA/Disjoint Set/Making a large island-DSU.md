# Making a large island
- You are given an n x n binary grid. A grid is said to be binary if every value in the grid is either 1 or 0. You can change at most one cell in the grid from 0 to 1. You need to find the largest group of connected  1's. Two cells are said to be connected if both are adjacent to each other and both have the same value.

### Logic
- Here, we have to change the value for each cell in the matrix and check for the largest island. We can say that, it is dynamically changing graph. And for dynamically changing graph, we make use of disjoint set.
- Here, the values in matrix have coordinates(row, col). So, in order to make use of disjoint set, we need to convert this coordinates into a single number.
- For that, index = row*n+col.
- Here, we need to return the size of largest component island, so, we make use of unionBySize
- For each change from 0 to 1, when we are calculating the size, there is a chance that cells of the same component might be present at 2 adjacent sides of the given cell. In such case, we might add the size 2 times. This is not correct. So, instead we add the ultimate parent of the adjacent cells in the set, to avoid the duplicacy.

### Solution
```
// User function Template for C++
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
    vector<int> dx = {0,0,1,-1};
    vector<int> dy = {1,-1,0,0};
    
    int largestIsland(vector<vector<int>>& grid) {
        // Your code goes here.
        int n = grid.size();
        DisjointSet ds(n*n);
        
        // lets join the island cells first
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]==0)
                {
                    continue;
                }
                for(int k=0;k<4;k++)
                {
                    int r = dx[k] + i;
                    int c = dy[k] + j;
                    if(r>=0 && r<n && c>=0 && c<n && grid[r][c]==1)
                    {
                        ds.unionBySize(i*n+j, r*n+c);
                    }
                }
            }
        }
        
        int mx=0;
        // if the cells have 0
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]==1)
                {
                    continue;
                }
                set<int> component;
                for(int k=0;k<4;k++)
                {
                    int r = dx[k] + i;
                    int c = dy[k] + j;
                    if(r>=0 && r<n && c>=0 && c<n && grid[r][c]==1)
                    {
                        component.insert(ds.findUPar(r*n+c));
                    }
                }
                int sizeTotal=0;
                for(auto it:component)
                {
                    sizeTotal+=ds.size[it];
                }
                mx=max(mx,sizeTotal+1);
                
            }
        }
        // if the matrix has all the 1's
        for(int i=0;i<n*n;i++)
        {
            mx = max (mx,ds.size[ds.findUPar(i)]);
        }
        return mx;
    }
};
```
- Time Complexity: O(n*n)
- Space Complexity: O(2*n*n) (parent and size array)

