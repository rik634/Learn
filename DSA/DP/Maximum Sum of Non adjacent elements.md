# Maximum sum of non adjacent elements
- We are given an array of N integers. We are supposed to return the maximum sum of the subsequence with the constraint that no 2 elements are adjacent in given array.

- Here, first thing we go for, is trying out all the subsequences and finding the one with maximum sum.
- The keyword "Trying out all ways" and "max value", leads to conclusion that this is DP problem.

### Recursion 

```
vector<int> arr;
maxSum(arr.size()-1);
int maxSum(int n)
{
    if(n<0)
    {
        return 0;
    }
    if(n==0)
    {
        return arr[0];
    }
    int pick = arr[n]+maxSum(n-2);
    int nonPick = maxSum(n-1);
    return max(pick, nonPick);
}
```
- Time Complexity: O(2^n)

### Memoization

```
vector<int> arr;
vector<int> dp(n,INT_MIN);
maxSum(n-1);
int maxSum(int n)
{
    if(n==0)
    {
        return arr[0];
    }
    if(n<0)
    {
        return 0;
    }
    if(dp[n]!=-1)
    {
        return dp[n];
    }
    int pick = arr[n]+maxSum(n-2);
    int nonPick = maxSum(n-1);
    
    return dp[n]=max(pick, nonPick);
}
```
- Time Complexity: O(N)
- Space Complexity: O(N) (array) + O(N) (stack space) = O(N)

### Tabulation

```

int maxSum(vector<int> arr, int n)
{
    vector<int> dp(n,INT_MIN);
    dp[0]=arr[0];
    dp[1]=max(arr[0],arr[1]);
    for(int i=2;i<n;i++)
    {
        dp[i]=max(dp[i-1],arr[i]+dp[i-2]);
    }
    return dp[n];
}
```
- Time Complexity: O(N)
- Space Complexity: O(N)


### Space Optimized
```

int maxSum(vector<int> arr, int n)
{
    
    int prev1=arr[0];
    int prev2=max(arr[0],arr[1]);
    int temp;
    for(int i=2;i<n;i++)
    {
        temp =max(prev2,arr[i]+prev1);
        prev1=prev2;
        prev2=temp;
    }
    return dp[n];
}
```
- Time Complexity: O(N)
- Space Complexity: O(N) 
