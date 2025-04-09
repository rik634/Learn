# Ninja's Training
- Ninja is planning this N day long training schedule. Each day, he can perform any one of these three activities (Running, Fighting Practice or Learning New Moves). Each activity has some merit points on each day. As ninja has to improve all his skills, he can;t do the same activity in 2 consecutive days. Can u help ninja find out the maximum merit points ninja can earn?
- You are given a 2D array of size N*3 Points with the points corresponding to each day and activity. Your task is to calculate the maximum number of merit points that ninja can earn.

## Logic:
- This seems to be greedy problem, but greedy fails here.
- So, try all possible ways --> Recursion
- To optimize recursion, to avoid recomputing the overlapping subproblem, we go for dynamic programming.

### Steps:
- Here, day will be our index.
- Here, along with the index, we also need to know which activity was performed on specific day. So, along with index, we have a parameter for that as well.
- we will pass that parameter with values (0,1,and 2) for the activities.

### Recursion


### Memoization



### Tabulation



### Space Optimized

