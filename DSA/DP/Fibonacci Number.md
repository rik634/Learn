# Fibonacci Numbers

### Recursion
```
int fib(int n)
{
    if(n==0||n==1)
    {
        return n;
    }
    return fib(n-1)+fib(n-2);
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
            res=n;
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
    f[0]=0;
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

### Space optimized
```
int fib(int n)
{
    int prev1=0;
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
- Dp is not best solution for Fibonacci problem. It can be further optimized to O(log N) time complexity.

