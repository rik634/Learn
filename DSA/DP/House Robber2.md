# House Robber
- You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

- Given an integer array "nums" representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

## Logic:
- Here, the solution for the problem (maximum sum with non adjacent elements) is not possible, as it can also contain both first and last element of the array.
- So, don't consider last element and find the sum, using the same solution.
similarly, don't consider first element, and find maximum sum.
- At last, take max of both the answers

### Recursion
### Memoization
### Tabulation
### Space Optimized

```

int maxS(vector<int> arr)
{
    
    int prev1=arr[0];
    int prev2=max(arr[0],arr[1]);
    int temp;
    for(int i=2;i<arr.size();i++)
    {
        temp =max(prev2,arr[i]+prev1);
        prev1=prev2;
        prev2=temp;
    }
    return dp[n];
}
int main()
{
    vector<int> nums;
    vector<int> temp1;
    vector<int> temp2;
    if(nums.size()==1)
    {
        return nums[0];
    }
    for(int i=0;i<nums.size();i++)
    {
        if(i!=0)
        {
            temp1.push_back(nums[i]);
        }
        if(i!=(n-1))
        {
            temp2.push_back(nums[i]);
        }
    }
    return max(maxS(temp1),maxS(temp2));
}

```
- Time Complexity: O(N)
- Space Complexity: O(N) 
