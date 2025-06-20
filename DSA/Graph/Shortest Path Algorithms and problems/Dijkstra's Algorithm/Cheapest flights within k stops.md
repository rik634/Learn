# Cheapest flights within K stops
- There are n cities connected by some number of flights. You are given an array flights where flights[i] = [from_i, to_i, price_i] indicates that there is a flight from city from_i to city toi with cost price_i.

- You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

### Logic
- If we try to use the Dijkstra's algorithm with priority queue. We will not get the feasible solution. As along with the goal of finding the cheapest price path, we have to take care of K stop condition. In case of Dijkstra, it gives priority to get the cheapest price, ignoring other solution with slightly greater price but satisfying the K stop condition. Because of slightly higher price, it ignores such possibilities as it is making use of priority queues. Here, we were using {dist, node , stops}
- So, our first priority of judgement has to be the number of stops, then we can go for price. Here, the queue will have {stops, node, dist}. Here, stop wise traversal happens, when the number of stops go beyond k, we can just stop adding the values in queue.
- Do not stop when reached the destination, as there is possibility to get the better solution for the further values in the queue.
- Here, we are using queue rather than priority queue, as at each step, we are increasing the stops by 1. So, by default, the values stored in queue are in the ascending order by values of stops.

### Solution
```
 int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        
        vector<pair<int,int>> adj[n];
        for(int i=0;i<flights.size();i++)
        {
            int u = flights[i][0];
            int v = flights[i][1];
            int p = flights[i][2];
            adj[u].push_back({v,p});
        }
        vector<int> dist(n,1e9);
        dist[src]=0;
        queue<pair<int,pair<int,int>>> q;
        q.push({0,{src,0}});
        
        while(!q.empty())
        {
            int size = q.size();
            
            for(int i=0;i<size;i++)
            {
                auto p = q.front();
                q.pop();
                int stops = p.first;
                int city = p.second.first;
                int price = p.second.second;
                if(stops>k)
                {
                    continue;
                }
                for(auto it:adj[city])
                {
                    if(dist[it.first]>(price+it.second) && stops<=k)
                    {
                        dist[it.first] = price+it.second;
                        q.push({stops+1,{it.first,dist[it.first]}});
                    }
                }
            }
        }
       if(dist[dst]==1e9)
       {
        return -1;
       }
       return dist[dst];
    }
```
- Time Complexity: O(E) (E = number of edges)
- If we use priority queue, it will be O(E*logV). As we are using queue, this additional "logV" is removed. Now, the time complexity is just "E"
- Space Complexity: O(|E|+|V|) (for adjacency list, queue, dist array)
