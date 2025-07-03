# Power set
- We are given an array of integers. We need to find power set (i.e., all the subsets) of the array.
- If the number of elements in the array is N, the number of subsets possible for that array is (2^N).
- So, size of the power set is (2^N).
- WE can write (2^N) also as (1 << N ).

### Logic
- For N elements in array, there are 2^N subsets possible. So, for each the value in the range 0 to (2^N)-1, there will be a corresponding binary representation. Depending on the bits, we will determine whether to include that element at that index or not. If the bit is set, we will include element at that index, if not, we will not include it.

### Solution
```
vector<vector<int>> powerSet(vector<int> nums)
{
    vector<vector<int>> ans;
    int n = nums.size();
    int subset = 1 << n ;
    for(int i=0;i<subset;i++)
    {
        vector<int> v;
        for(int j=0;j<n;j++)
        {
            if(i & (1<< i) == 1)
            {
                v.push_back(nums[j]);
            }
        }
        ans.push_back(v);
    }
    return ans;
}
```
- Time Complexity: O((2^n) * n)
- Space Complexity: O((2^n)*n) (total subsets stores in final 2d array, and in worst case, each of the subsets will be of size n)
