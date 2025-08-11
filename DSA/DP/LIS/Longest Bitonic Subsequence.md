# Longest Bitonic Subsequence:
- A bitonic subsequence is a subsequence of an array in which the elements can be any of these three:

  1. First, increase till a point and then decrease.
  2. Goes on increasing (Longest increasing subsequence)
  3. Goes on decreasing (Longest decreasing subsequence)

- Find the length of the longest bitonic subsequence in an array.   

### Solution
```cpp
int longestBitonicSequence(vector<int>& arr, int n) {
    
    vector<int> dp1(n, 1); 
    vector<int> dp2(n, 1); 
   
    for (int i = 0; i < n; i++) {
        for (int prev_index = 0; prev_index < i; prev_index++) {
            if (arr[prev_index] < arr[i]) {
                dp1[i] = max(dp1[i], 1 + dp1[prev_index]);
            }
        }
    }

    for (int i = n - 1; i >= 0; i--) {
        for (int prev_index = n - 1; prev_index > i; prev_index--) {
            if (arr[prev_index] < arr[i]) {
                dp2[i] = max(dp2[i], 1 + dp2[prev_index]);
            }
        }
    }

    int maxi = -1;
    for (int i = 0; i < n; i++) {
        maxi = max(maxi, dp1[i] + dp2[i] - 1);
    }

    return maxi;
}

```
- Time Complexity: O(n^2)
- Space Complexity: O(n) for the dp arrays.


### Solution: (subsequence is not strictly increasing or decreasing)
```cpp
int LongestBitonicSequence(int n, vector<int> nums) {
        vector<int> lis(n, 1);
        /* Compute LIS values from left to right */
        for (int i = 1; i < n; i++)
            for (int j = 0; j < i; j++)
                if (nums[i] > nums[j] and lis[i] < lis[j] + 1)
                    lis[i] = lis[j] + 1; // updating LIS value for current index

        vector<int> lds(n, 1);
        /* Compute LDS values from right to left */
        for (int i = n - 2; i >= 0; i--)
            for (int j = n - 1; j > i; j--)
                if (nums[i] > nums[j] and lds[i] < lds[j] + 1)
                    lds[i] = lds[j] + 1; // updating LDS value for current index

        /* Return the maximum value of lis[i] + lds[i] - 1*/
        int ans = 0; // initializing ans with length of longest bitonic sequence
        for (int i = 1; i < n - 1; i++)
            if (lis[i] > 1 && lds[i] > 1) {
                ans = max(ans,
                          lis[i] + lds[i] - 1); // updating ans if current index has
                                                // non-zero values for both LIS and LDS
            }
        return ans;
    }
```
- Time Complexity: O(n^2)
- Space Complexity: O(n) for the dp arrays.
