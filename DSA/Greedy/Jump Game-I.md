# Jump Game
- Given an array where each element represents the maximum number of steps you can jump forward from that element, return true if we can reach the last index starting from the first index. Otherwise, return false.

### Logic
- If there is 0 in the array, we might or might not be able to reach the end. While, if the array does not contain 0, we will definitely reach the end.

### Solution
```
bool canJump(vector<int>& nums) {
        
        int n = nums.size();
        int maxIndex=0;
        for(int i=0;i<n;i++)
        {
            if(i>maxIndex)
            {
                return false;
            }
            maxIndex=max(maxIndex,i+nums[i]);
        }
        return true;
    }
```
- Time Complexity: O(N)
- Space Complexity: O(1)