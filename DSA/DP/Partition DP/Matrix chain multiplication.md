# Matrix chain multiplication
- Given an array arr[] which represents the dimensions of a sequence of matrices where the ith matrix has the dimensions (arr[i-1] x arr[i]) for i>=1, find the most efficient way to multiply these matrices together. The efficient way is the one that involves the least number of multiplications.

### Pattern identification:
- Whenever we need to find the answer to a large problem such that the problem can be broken into subproblems and the final answer varies due to the order in which the subproblems are solved, we can think in terms of partition DP.

### Steps:
1. Start with entire block/array. Represent the block with a pair of indices (i, j) where i is the starting index and j is the ending index.
2. For each pair of indices (i, j), try to partition the block at every possible index k such that i <= k < j. This means we will try to split the block into two parts: (i, k) and (k+1, j).   
3. For each partition, calculate the cost of multiplying the two parts together and add it to the cost of multiplying the resulting matrices.       
4. Keep track of the minimum cost encountered while trying all possible partitions.
- The cost of multiplying two matrices A (of dimensions p x q) and B (of dimensions q x r) is given by p * q * r. In our case, the dimensions of the matrices are determined by the values in the arr[] array.

### Logic:
- f(i,j) = f(1,n-1) = Returns the minimum number of multiplications needed to multiply matrices from index i to j.

### Recursion:
```cpp
int f(int l, int r, vector<int>& arr)
    {
        if(l==r)
        {
            return 0;
        }
        int mn=1e9;
        for(int k=l;k<=r-1;k++)
        {
            int steps = arr[l-1]*arr[k]*arr[r] + f(l,k,arr)+ f(k+1,r,arr);
            mn=min(mn,steps);
        }
        return mn;
    }
    int matrixMultiplication(vector<int> &arr) {
        // code here
        int n = arr.size();
        return f(1,n-1,arr);
    }
```
- Time Complexity: O(n^3)
- Space Complexity: O(1) 

### Memoization:
```cpp
 int f(int l, int r, vector<int>& arr,vector<vector<int>>& dp)
    {
        if(l==r)
        {
            return 0;
        }
        if(dp[l][r]!=-1)
        {
            return dp[l][r];
        }
        int mn=1e9;
        for(int k=l;k<=r-1;k++)
        {
            int steps = arr[l-1]*arr[k]*arr[r] + f(l,k,arr,dp)+ f(k+1,r,arr,dp);
            mn=min(mn,steps);
        }
        return dp[l][r]=mn;
    }
    int matrixMultiplication(vector<int> &arr) {
        // code here
        int n = arr.size();
        vector<vector<int>> dp(n,vector<int>(n,-1));
        return f(1,n-1,arr,dp);
    }
```
- Time Complexity: O(n^3)
- Space Complexity: O(n^2) for the dp array + O(n) (auxiliary stack space)

- The order of changing states in case of tabulation is in the opposite order to that of in recursion or memoization. Let's say, here, in memoization, the value of i starts from 1 till N-1. So, in tabulation, value of i will start from N-1 till 1. The value of j will always be greater than i, so it will start from i+1 till N.


### Tabulation:
```cpp  
 int matrixMultiplication(vector<int> &arr) {
        // code here
        int n = arr.size();
        vector<vector<int>> dp(n,vector<int>(n,0));
        for(int i=n-1;i>0;i--)
        {
            for(int j=i+1;j<n;j++)
            {
                int mn=1e9;
                for(int k=i;k<=j-1;k++)
                {
                    int steps = arr[i-1]*arr[k]*arr[j] + dp[i][k]+ dp[k+1][j];
                    mn=min(mn,steps);
                }
                dp[i][j]=mn;
            }
        }
        return dp[1][n-1];
    }
```
- Time Complexity: O(n^3)
- Space Complexity: O(n^2) for the dp array
- The space complexity can be reduced to O(n) by using a single array to store the results of the last computed row, as we only need the previous row's results to compute the current row.

### Space optimization:
```cpp
 int matrixMultiplication(vector<int> &arr) {
        // code here
        int n = arr.size();
        vector<int> dp(n,0);
        for(int i=n-1;i>0;i--)
        {
            vector<int> temp(n,0);
            for(int j=i+1;j<n;j++)
            {
                int mn=1e9;
                for(int k=i;k<=j-1;k++)
                {
                    int steps = arr[i-1]*arr[k]*arr[j] + dp[k]+ temp[j];
                    mn=min(mn,steps);
                }
                temp[j]=mn;
            }
            dp=temp;
        }
        return dp[n-1];
    }
``` 
- Time Complexity: O(n^3)
- Space Complexity: O(n) for the dp array   