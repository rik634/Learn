# Minimum Multiplications to reach end
- Given start, end, and an array arr of n numbers. At each step, the start is multiplied by any number in the array and then a mod operation with 100000 is done to get the new start.
- Your task is to find the minimum steps in which the end can be achieved starting from the start. If it is not possible to reach the end, then return -1

### Logic
- Here, we need to find the min steps to reach the destination. we need to use Dijkstra's algorithm. 
- As at each level, number of stops is increasing by 1, we don't need to use the priority queue, and can go with queue itself.


### Solution
```
 int minimumMultiplications(vector<int>& arr, int start, int end) {
        
        queue<pair<int, int>> q;
        q.push({start, 0});

        
        vector<int> dist(100000, 1e9);
        dist[start] = 0;
        int mod = 100000;

        if(start==end)
        {
            return 0;
        }
        while (!q.empty())
        {
            int node = q.front().first;
            int steps = q.front().second;
            q.pop();

            for (auto it : arr)
            {
                int num = (it * node) % mod;

                
                if (steps + 1 < dist[num])
                {
                    dist[num] = steps + 1;

                    
                    if (num == end)
                        return steps + 1;
                    q.push({num, steps + 1});
                }
            }
        }
       
        return -1;
    }
```
- Time Complexity: O(100000*N)
- Space Complexity: O(100000*N)