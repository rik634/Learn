# Bus Routes
- You are given an array routes representing bus routes where routes[i] is a bus route that the ith bus repeats forever.

- For example, if routes[0] = [1, 5, 7], this means that the 0th bus travels in the sequence 1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ... forever.
You will start at the bus stop source (You are not on any bus initially), and you want to go to the bus stop target. You can travel between bus stops by buses only.

- Return the least number of buses you must take to travel from source to target. Return -1 if it is not possible.

### Logic:
- Similar to finding the shortest distance between source and the target

### Companies
- Uber

### BFS
```
int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        
        unordered_map<int,vector<int>> bStopTobIds;
        for(int i=0;i<routes.size();i++)
        {
            int bId = i;
            for(auto it:routes[i])
            {
                bStopTobIds[it].push_back(i);
            }
        }
        queue<pair<int,int>> q; // current stop, number of buses travelled
        q.push({source,0});
        unordered_map<int,bool> bVis, bStopVis;
        bStopVis[source]=true;
        while(!q.empty())
        {
           int size = q.size();
           for(int i=0;i<size;i++)
           {
                auto p = q.front();
                q.pop();
                int cStop = p.first;
                int busTravelled = p.second;
                if(cStop==target)
                {
                    return busTravelled;
                }
                for(auto bus: bStopTobIds[cStop])
                {
                    if(bVis[bus]==true)
                    {
                        
                        continue;
                    }
                    bVis[bus]=true;
                    for(auto busStop: routes[bus])//check busstops for not visites bus ids
                    {
                        
                        if(bStopVis[busStop]==false)
                        {
                            bStopVis[busStop]=true;
                            q.push({busStop,busTravelled+1});
                        }
                        else
                        {
                            bStopVis[busStop]=true;
                            continue;
                        }
                    }
                }
           }


        }
        return -1;
    }
```
- Time Complexity : O(n*m) (n=number of buses, m = number of stops each bus can have)
- Space Complexity: O(n*m)