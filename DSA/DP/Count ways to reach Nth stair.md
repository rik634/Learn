# Count ways to reach Nth stair
- We have given a number of stairs. Initially, we are at 0th stair, and we need to reach the Nth stair. Each time we can either climb one step or two steps. We are supposed to return the number of distinct ways in which we can climb from 0th step to the Nth step.

### Recursion
```
int count(int n)
{
    if(n<=1)
    {
        return 1;
    }
    return count(n-1)+count(n-2);
}
```
- Time Complexity: O(2^n)


### Memoization
```
int n;
vector<int> memo(n+1,-1);
int fib(int n)
{
    if(memo[n]==-1)
    {
        int res;
        if(n==0||n==1)
        {
            res=1;
        }
        else
        {
            res = fib(n-1)+fib(n-2);
        }
        memo[n]=res;
    }
    return memo[n];
}
```
- Time Complexity: theta(n)
- Space complexity: theta(N)


### Tabulation
```
int fib(int n)
{
    int f[n+1];
    f[0]=1;
    f[1]=1;
    for(int i=2;i<=n;i++)
    {
        f[i]=f[i-1]+f[i-2];
    }
    return f[n];
}
```
- Time Complexity: theta(N)
- Space Complexity: theta(N)

### Space Optimized
```
int fib(int n)
{
    int prev1=1;
    int prev2=1;
    if(n<=1)
    {
        return n;
    }
    int temp;
    for(int i=2;i<=n;i++)
    {
        temp = prev1+prev2;
        prev1=prev2;
        prev2=temp;
    }
    return temp;
}
```
- Time complexity: O(N)
- Space Complexity: O(1)