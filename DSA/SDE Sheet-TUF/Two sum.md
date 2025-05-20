# two Sum
- Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

## Brute Force
```
class Solution {
public:
    vector<int> twoSum(vector<int> &nums, int target) {
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[j] == target - nums[i]) {
                    return {i, j};
                }
            }
        }
        // Return an empty vector if no solution is found
        return {};
    }
};
```
- Time Complexity: O(n^2)
- Space Complexity: O(1)

### Hash Table
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        
        unordered_map<int,int> mp;
        vector<int> res;
        for(int i=0;i<nums.size();i++)
        {
            int num = target-nums[i];
            if(mp.find(num)!=mp.end() && mp[num]!=i)
            {
                res.push_back(i);
                res.push_back(mp[num]);
                return res;
            }
            if(mp.find(nums[i])==mp.end())
            {
                mp.insert({nums[i],i});
            }
            
        }
        return res;
    }
};
```
- Time Complexity: O(n)
- Space Complexity: O(n)

### 