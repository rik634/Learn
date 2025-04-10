# Ninja's Training
- Ninja is planning this N day long training schedule. Each day, he can perform any one of these three activities (Running, Fighting Practice or Learning New Moves). Each activity has some merit points on each day. As ninja has to improve all his skills, he can;t do the same activity in 2 consecutive days. Can u help ninja find out the maximum merit points ninja can earn?
- You are given a 2D array of size N*3 Points with the points corresponding to each day and activity. Your task is to calculate the maximum number of merit points that ninja can earn.

## Logic:
- This seems to be greedy problem, but greedy fails here.
- So, try all possible ways --> Recursion
- To optimize recursion, to avoid recomputing the overlapping subproblem, we go for dynamic programming.

### Steps:
- Here, day will be our index.
- Here, along with the index, we also need to know which activity was performed on previous day. So, along with index, we have a parameter for that as well.
- we will pass that parameter with values (0,1,and 2) for the activities.
- for the start day, lane is passed with value 3, means, we can select any one of the 3 lanes


### Recursion
```
int f(day, lane)
{
    int mx=0;
    if(day==0)
    {
        mx=0;
        for(int i=0;i<=2;i++)
        {
            if(i!=lane)
            {
                mx=max(mx,task[day][i]);
            }
        }
        return mx;
    }
    mx=0;
    for(int i=0;i<=2;i++)
    {
        if(i!=last)
        {
           mx = max(mx,task[day][i]+f(day-1,i));
        }
    }
    return mx;
}
int main()
{
    f(n-1,3);
}
```
- Time Complexity: O(2^n)

- Here, If we draw recursion tree, we can observer the overlapping subproblem. So, to optimize this, we go for DP.
- Here, we are passing 2 parameters to the recursion function. There are 2 parameters which are changing the values.
- So, the DP array being used will be a 2D array.
- Now, the parameters are day, and lane.
- possible values for day are : {0 to n-1}. So, size of day= n
- possible vales for lane are: {0,to 3}. So, size of lane= 4
```
vector<vector<int>> dp(n,vector<int>(4,-1));
```


### Memoization
```
vector<vector<int>> dp(n,vector<int>(4,-1));
int f(day, lane)
{
    int mx=0;
    if(day==0)
    {
        mx=0;
        for(int i=0;i<=2;i++)
        {
            if(i!=lane)
            {
                mx=max(mx,task[day][i]);
            }
        }
        return mx;
    }
    if(dp[day][last]!=-1)
    [
        return dp[day][last];
    ]
    mx=0;
    for(int i=0;i<=2;i++)
    {
        if(i!=last)
        {
           mx = max(mx,task[day][i]+f(day-1,i));
        }
    }
    return dp[day][last]=mx;
}
int main()
{
    f(n-1,3);
}
```
- Time Complexity: O(4*N)*3 (Multiplied by 3, because, for every day/index, we are running a for loop of length 3).

- Space Complexity: O(4*N) (auxiliary space) + O(N) (stack space, as max number of days we will go for is N)

### Tabulation
```
int ninjaTraining(vector<vector<int>> points)
{
    vector<vector<int>> dp(n,vector<int>(4,0));
    dp[0][0]=max(points[0][1],points[0][2]);
    dp[0][1]=max(points[0][0],points[0][2]);
    dp[0][2]=max(points[0][1],points[0][0]);
    dp[0][3]=max(points[0][1],max(points[0][2],points[0][0]));
    for(int i=1;i<n;i++)
    {
        for(int j=0;j<4;j++)
        {
            int mx=0;
            for(int l=0;l<3;l++)
            {
                if(l!=j)
                {
                    mx=max(mx,points[i][l]+dp[i-1][l]);
                }
            }
            dp[i][j]=mx;
        }
    }
    return dp[n-1][3];
}

```
- Time Complexity: O(N*4*3)
- Space Complexity: O(N*4)

### Space Optimized
```
int ninjaTraining(int n, vector<vector<int>>& points) {
 
  vector<int> prev(4, 0);

  prev[0] = max(points[0][1], points[0][2]);
  prev[1] = max(points[0][0], points[0][2]);
  prev[2] = max(points[0][0], points[0][1]);
  prev[3] = max(points[0][0], max(points[0][1], points[0][2]));

 
  for (int day = 1; day < n; day++) {
   
    vector<int> temp(4, 0);
    for (int last = 0; last < 4; last++) {
      temp[last] = 0;
      for (int task = 0; task <= 2; task++) {
        if (task != last) {
         
          temp[last] = max(temp[last], points[day][task] + prev[task]);
        }
      }
    }
   
    prev = temp;
  }
  return prev[3];
}
```
- Time Complexity: O(N*4*3)
- Space Complexity: O(4)