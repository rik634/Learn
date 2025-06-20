# Number of ways to arrive at destination
 - You are in a city that consists of n intersections numbered from 0 to n - 1 with bi-directional roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

- You are given an integer n and a 2D integer array ‘roads’ where roads[i] = [u_i, v_i, time_i] means that there is a road between intersections ui and vi that takes time_i minutes to travel. You want to know in how many ways you can travel from intersection 0 to intersection n - 1 in the shortest amount of time.

- Return the number of ways you can arrive at your destination in the shortest amount of time. Since the answer may be large, return it modulo 109 + 7.

### Logic
- Number of ways to arrive at any node is the summation of the number of wasy to arrive at the parent nodes for this node.
- Here, we will be implementing the Dijkstra's Algorithm to get the shortest time between source and destination. As well as the number of ways to arrive at particular node in shortest time. 
- Here, we will use Priority queue, dist array, ways array


### Solution
```
 int countPaths(int n, vector<vector<int>>& roads) {
        
        vector<pair<int,int>> adj[n];
        long long mod = (long long)(1e9 + 7);
        //undirected graph
        for(int i=0;i<roads.size();i++)
        {
            int u = roads[i][0];
            int v = roads[i][1];
            int wt = roads[i][2];
            adj[u].push_back({v,wt});
            adj[v].push_back({u,wt});
        }
        vector<long long> time(n,LLONG_MAX);
        vector<int> ways(n,0);
        priority_queue<pair<long long, long long>,vector<pair<long long, long long>>, greater<pair<long long, long long>>> q;
        time[0]=0;
        ways[0]=1;
        q.push({0,0});
        while(!q.empty())
        {
            auto p = q.top();
            q.pop();
            long long timeTravelled = p.first;
            long long city = p.second;
            for(auto it:adj[city])
            {
                long long node = it.first;
                long long timeRequired = it.second;
                if(timeRequired>time[node])
                {
                    continue;
                }
                else if(time[node]>(timeTravelled+timeRequired))
                {
                    time[node]=timeTravelled + timeRequired;
                    q.push({time[node],node});
                    ways[node]=ways[city];
                }
                else if(time[node]==(timeTravelled+timeRequired))
                {
                    ways[node] = (ways[node]+ways[city])%mod;
                }
                else
                {
                    continue;
                }

            }

        }
        return ways[n-1]% mod;
    }
```
- Time Complexity: O(E*logV)
- Space Complexity: O(V) (dist array, ways array, priority queue space )