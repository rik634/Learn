# Jump Game-II
- You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

- Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:
  1. 0 <= j <= nums[i] and
  2. i + j < n

- Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

### Logic


### Recursive solution:
```
int f(int ind, int jumps)
{
    if(ind>=(n-1))
    {
        return jumps;
    }
    int mn=INT_MAX;
    for(int i=1;i<=arr[ind],i++)
    {
        mn=min(mn,f(ind+i,jumps+1));
    }
    return mn;
}
```
- Time Complexity: O(N^N) (for every index, we have at most N possibilities. So, the base will be N. At max, we have to find this for N indices, so, the power will be N.)
- Space Complexity: O(N) (recursive stack space)

### Memoization
```
vector<vector<int>> dp(N,vector<int>(N,-1));
int f(int ind, int jumps)
{
    if(ind>=(n-1))
    {
        return jumps;
    }
    if(dp[ind][jumps]!=-1)
    {
        return dp[ind][jumps];
    }
    int mn=INT_MAX;
    for(int i=1;i<=arr[ind];i++)
    {
        mn=min(mn,f(ind+i,jumps+1));
    }
    return dp[ind][jumps]=mn;
}
```
- Time complexity: O(N*N) // we will be calling this function N*N times only to fill the dp array. If the value is already present, we will return the same.
- Space complexity: O(N*N)

### More optimized
```
int jumpGame(vector<int> arr)
{
    int jumps=0;
    int l=0,r=0;
    int n = arr.size();
    while(r<(n-1))
    {
        int farthest=0;
        for(int i=l;i<=r;i++)
        {
            farthest=max(i+arr[i],farthest);
        }
        l=r+1;
        jumps++;
        r=farthest;
    }
    return jumps;
}
```
- Time Complexity: O(N)
- Space Complexity: O(1)
