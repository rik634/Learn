# Course Schedule I
- There are a total of numCourses courses you have to take, labeled from 0 to numCourses  - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

- For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
- Return true if you can finish all courses. Otherwise, return false.

### Logic
- use topo sort of cycle detection algorithm using DFS

### BFS
```
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        
        vector<int> adj[numCourses];
        vector<int> indegree(numCourses,0);
        for(int i=0;i<prerequisites.size();i++)
        {
            adj[prerequisites[i][1]].push_back(prerequisites[i][0]);
            indegree[prerequisites[i][0]]++;
        }
        queue<int> q;
        for(int i=0;i<numCourses;i++)
        {
            if(indegree[i]==0)
            {
                q.push(i);
            }
        }
        int count=0;
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            count++;
            for(auto it: adj[p])
            {
                indegree[it]--;
                if(indegree[it]==0)
                {
                    q.push(it);
                }
            }
        }
        return count==numCourses;
    }
```
- Time Complexity: O(V+E)
- Space Complexity: O(V+E) (adjacency list) + O(V) (indegree vector)

### DFS
```
bool dfs(int node, vector<bool>& vis, vector<int> adj[], vector<bool>& pathVis)
    {
        vis[node]=true;
        pathVis[node]=true;
        for(auto it: adj[node])
        {
            if(vis[it]==false)
            {
                if(dfs(it,vis,adj,pathVis)==true)
                {
                    return true;
                }
            }
            else if(pathVis[it]==true)
            {
                return true;
            }
        }
        pathVis[node]=false;
        return false;

    }
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        
        vector<int> adj[numCourses];
        
        for(int i=0;i<prerequisites.size();i++)
        {
            adj[prerequisites[i][1]].push_back(prerequisites[i][0]);
        }
        vector<bool> vis(numCourses,false);
        vector<bool> pathVis(numCourses,false);
        for(int i=0;i<numCourses;i++)
        {
            if(vis[i]==false)
            {
                if(dfs(i,vis,adj,pathVis)==true)
                {
                    return false;
                }
            }
        }
        return true;
    }
```
- Time Complexity: O(V+E)
- Space Complexity: O(2*V) + O(V) (recursive stack space)
 

# Course Schedule II
- There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

- For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
- Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

### Logic

### BFS
```
 vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        
        vector<int> adj[numCourses];
        vector<int> indegree(numCourses,0);
        for(int i=0;i<prerequisites.size();i++)
        {
            adj[prerequisites[i][1]].push_back(prerequisites[i][0]);
            indegree[prerequisites[i][0]]++;
        }
        queue<int> q;
        for(int i=0;i<numCourses;i++)
        {
            if(indegree[i]==0)
            {
                q.push(i);
            }
        }
        vector<int> res;
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
           res.push_back(p);
            for(auto it: adj[p])
            {
                indegree[it]--;
                if(indegree[it]==0)
                {
                    q.push(it);
                }
            }
        }
        if(res.size()==numCourses)
        {
            return res;
        }
        return {};
    }
```
- Time Complexity: O(V+E)
- Space Complexity: O(V) + O(V) + O(V)

### DFS
```
 bool dfs(int node, vector<bool>& vis, stack<int>& s, vector<int> adj[],vector<bool>& pathVis)
    {
        vis[node]=true;
        pathVis[node]=true;
        
        for(auto it: adj[node])
        {
            if(vis[it]==false)
            {
                if(dfs(it,vis,s,adj,pathVis)==true)
                {
                    return true;
                }
            }
            else if(pathVis[it]==true)
            {
                return true;
            }
        }
        pathVis[node]=false;
        s.push(node);
        return false;
    }
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> adj[numCourses];
        for(int i=0;i<prerequisites.size();i++)
        {
            adj[prerequisites[i][1]].push_back(prerequisites[i][0]);
        }
        vector<bool> vis(numCourses,false);
        vector<bool> pathVis(numCourses,false);
        stack<int> s;
        for(int i=0;i<numCourses;i++)
        {
            if(vis[i]==false)
            {
                if(dfs(i,vis,s,adj,pathVis)==true)
                {
                    return {};
                }
            }
        }
        vector<int> res;
        while(!s.empty())
        {
            auto it = s.top();
            s.pop();
            res.push_back(it);
        }
        if(res.size()==numCourses)
        {
            return res;
        }
        return {};
    }
```
- Time Complexity: O(V+E) + O(V)
- Space Complexity: O(V) + O(V) + O(V)