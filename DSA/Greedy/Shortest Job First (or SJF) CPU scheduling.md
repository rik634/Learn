# Shortest Job First (or SJF) CPU scheduling
-  Given a list of job durations representing the time it takes to complete each job. Implement the Shortest Job First algorithm to find the average waiting time for these jobs.

### Logic
- we are given an array, which contains the execution tine of every job.
- we need to fin the average waiting time for all the processes following the shortest job first algorithm.
- Shortest job first (SJF) algorithm is the scheduling policy that selects the waiting process with the smallest execution time to execute next.
- Here, the job with least execution time will be scheduled to execute first.
 

### Solution
```
long long solve(vector<int>& bt) {
        // code here
        sort(bt.begin(),bt.end());
        
        int prev=0;
        int temp =0;
        int total=0;
        for(int i=0;i<bt.size();i++)
        {
           
            total+=temp;
            temp = prev+bt[i];
            prev=temp;
        }
        return floor(total/bt.size());
        
    }
```
- Time Complexity: O(N) + O(N*logN)
- Space Complexity: O(1)