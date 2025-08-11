# Number of longest increasing subsequences
- Given an integer array nums, return the number of longest increasing subsequences.
- Notice that the sequence has to be strictly increasing.


### Solution
```cpp
 int findNumberOfLIS(vector<int>& nums) {
        
        int n = nums.size();
        vector<int> dp(n,1);
        vector<int> count(n,1);
        int mx=1;
        int k=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<i;j++)
            {
                if(nums[i]>nums[j] && dp[i]<(1+dp[j]))
                {
                    dp[i]=1+dp[j];
                    count[i]=count[j];
                }
                else if(nums[i]>nums[j] && dp[i]==1+dp[j])
                {
                    count[i]+=count[j];
                }
            }
           mx=max(mx,dp[i]);
        }
        for(int i=0;i<n;i++)
        {
            if(dp[i]==mx)
            {
                k+=count[i];
            }
        }
        return k;
        
    }
```
- Time Complexity: O(n^2)
- Space Complexity: O(n) for the dp and count arrays.   