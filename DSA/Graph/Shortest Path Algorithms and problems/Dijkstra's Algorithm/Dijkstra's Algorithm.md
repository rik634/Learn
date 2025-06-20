# Dijkstra's Algorithm
- Given a weighted, undirected, and connected graph of V vertices and an adjacency list adj where adj[i] is a list of lists containing two integers where the first integer of each list j denotes there is an edge between i and j, second integers corresponds to the weight of that edge. You are given the source vertex S and You have to Find the shortest distance of all the vertex from the source vertex S. You have to return a list of integers denoting the shortest distance between each node and the Source vertex S.

- Note: The Graph doesn’t contain any negative weight cycle.

- This algorithm can be implemented in 3 ways, using:
1. Priority Queue
2. Set (fastest)
3. Queue (Very slow, takes too much time)

- Dijkstra's algorithm is not applicable for:
1. The graphs with negative edge weights: For the negative weights, it might end up in infinite loop
2. The graphs which end up having the negative weight cycle
3. 

### Logic


### Using Priority Queue
```
vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
        // Code here
        
        
        vector<int> dist(V,1e9);
        dist[src]=0;
        //we need min heap storing the pair of (distance, node)
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> q;
        q.push({0,src});
        //create adjacency list
        vector<pair<int,int>> adj[V];
        for(int i=0;i<edges.size();i++)
        {
            int u = edges[i][0];
            int v = edges[i][1];
            int wt = edges[i][2];
            adj[u].push_back({v,wt});
        }
        while(!q.empty())
        {
            auto p = q.top();
            q.pop();
            int distance = p.first;
            int node = p.second;
            for(auto it:adj[node])
            {
                if(dist[it.first]>(distance+it.second))
                {
                    dist[it.first] = distance + it.second;
                    q.push({dist[it.first],it.first});
                }
            }
        }
        return dist;
    }
```
- Time Complexity:O(E*logV) (E=Number of edges, V = Number of nodes)
- Space Complexity:O(|E|+|V|)

### Using Set

- As we know, set stores the values in ascending order.

```
vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
        // Code here
        vector<int> dist(V,1e9);
        dist[src]=0;
        //we need min heap storing the pair of (distance, node)
        set<pair<int,int>> s;
        s.insert({0,src});
        //create adjacency list
        vector<pair<int,int>> adj[V];
        for(int i=0;i<edges.size();i++)
        {
            int u = edges[i][0];
            int v = edges[i][1];
            int wt = edges[i][2];
            adj[u].push_back({v,wt});
        }
        while(!s.empty())
        {
            auto p = *(s.begin());
            s.erase(p);
            int distance = p.first;
            int node = p.second;
            for(auto it:adj[node])
            {
                if(dist[it.first]>(distance+it.second))
                {
                    //erase if the distance is not 1e9, means we had already got to this node, 
                    //and now we are getting lesser distance than earlier one
                    if(dist[it.first]!=1e9)
                    {
                        s.erase({dist[it.first],it.first});
                    }
                    dist[it.first] = distance + it.second;
                    s.insert({dist[it.first],it.first});
                }
            }
        }
        return dist;
    }
```
- Time Complexity:O(E*logV)  (E=Number of edges, V = Number of nodes)
- Space Complexity:O(|E|+|V|)

### Tradeoff between the use of priority queue and sets:

- Why do we prefer to use priority queue instead of queue data structure?
Answer: By making use of priority queue, we first traverse the min distance first, which saves our time. In case of queue, we get the answer eventually, but it takes a lot of time. So, the priority queue is preferred over queue. And set is preferred over priority queue.

- Time Complexity calculation: O(E*LogV)
Answer:
- Here, the while loop can iterate max V times.
- To get the top element in min heap will take: log(heap size)
- In case of "for" loop, there can be max of V-1 nodes in the adjacency list for every node, in case of dense graph in worst scenario: V-1
- In the for loop, we are pushing the value into min heap: Log(heap size)
- In the worst case, for the dense graph, the max heap size can be: V^2
- For the case of dense graph, each of the V nodes, will be connected to the rest of thr V-1 nodes. So, approximately V^2 is equal to the number of edges, E.
Time Complexity: V*(log(heap size) + (V-1)*(log(heap size))) = V*V*log(heap size) = V^2*(logV) = E*(logV)
- So, our time complexity for this Dijkstra's Algorithm is: O(E*Log(V))
