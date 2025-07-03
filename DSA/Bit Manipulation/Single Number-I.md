# Single Number-I
- WE are given array, where every elements appears twice except for one element. Return the element appearing once.

### Logic
- a^a =0
- a^0 = a

### Solution
```
int singleNumber(vector<int> nums)
{
    int xor =0;
    for(int i=0;i<nums.size();i++)
    {
        xor = xor ^ nums[i];
    }
    return xor;
}
```
- Time Complexity: O(N)
- Space Complexity: O(1)