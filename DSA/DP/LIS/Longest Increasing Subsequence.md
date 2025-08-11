# Longest Increasing Subsequence
-  Subsequence of a sequence can be obtained by picking 0 or more elements from a sequence
- Picked elements in the subsequence should appear in the same order as they appear in the original sequence
- If n is the length of any sequence. Then there is total 2^n total possible subsequence.


### Naive solution:
- Here, we need to get the all the possible subsequences. It can be done either using power set or recursion.
- Find which of them is increasing subsequence. And then the one with the maximum length is the answer.

### steps for recursion:
1. Express everything in terms of index (ind, prev).
2. Explore all possibilities, (take, notTake).
3. Take the max length of (take, notTake).
4. base case.

- f(0,-1): Give me the length of LIS starting from 0th index, with no previous index. 
- f(3,0): Length of LIS starting from 3rd index, whose previous index is 0th index.

### Recursion:
```cpp
int f(int ind, int prev, vector<int>& nums)
    {
        if(ind==nums.size())
        {
            return 0;
        }
        int notTake = 0 + f(ind+1,prev,nums);
        int take=0;
        if(prev==-1 || nums[ind]>nums[prev])
        {
            take = 1 + f(ind+1,ind,nums);
        }
        return max(take, notTake);
    }
    int lengthOfLIS(vector<int>& nums) {
        
        int n = nums.size();
        return f(0,-1,nums);
    }
```
- Time Complexity: O(2^n) (for each element, we have two choices: take or not take)
- Space Complexity: O(n) (for the recursion stack)  

- as we have the overlapping subproblems, we can convert it into memoization.
- for "ind", max size is N. for "prev", its range is -1 to n-1. we can have the index -1, in the 2d array, so, we do coordinate shift. and we will take the range from 0 to n, if size n+1.

### Memoization:
```cpp
int f(int ind, int prev, vector<int>& nums, vector<vector<int>>& dp)
{
    if(ind==nums.size())
    {
        return 0;
    }
    if(dp[ind][prev+1]!=-1)
    {
        return dp[ind][prev+1];
    }
    int notTake = 0 + f(ind+1,prev,nums,dp);
    int take=0;
    if(prev==-1 || nums[ind]>nums[prev])
    {
        take = 1 + f(ind+1,ind,nums,dp);
    }
    return dp[ind][prev+1] = max(take, notTake);
}
int lengthOfLIS(vector<int>& nums) {

    int n = nums.size();
    vector<vector<int>> dp(n, vector<int>(n+1, -1));
    return f(0,-1,nums,dp);
}
```
- Time Complexity: O(n*(n+1)) (as we are storing the results of subproblem)
- Space Complexity: O(n*(n+1)) + O(n) (auxiliary space for recursion stack) = O(n^2) (for the dp array)

### tabulation (DP)
```cpp
    int lengthOfLIS(vector<int>& nums) 
    {
        int n = nums.size();
        vector<vector<int>> dp(n+1, vector<int>(n+1, 0));
        for(int ind=n-1;ind>=0;ind--)
        {
            for(int prev=ind-1;prev>=-1;prev--)
            {
                int notTake = 0 + dp[ind+1][prev+1];
                int take=0;
                if(prev==-1 || nums[ind]>nums[prev])
                {
                    take = 1 + dp[ind+1][ind+1];
                }
                dp[ind][prev+1] = max(take, notTake);
            }
        }
        return dp[0][0];
    }
```
- Time Complexity: O(n^2) (as we are storing the results of subproblem)
- Space Complexity: O(n^2) (for the dp array)   

### Space Optimized
- We can optimize the space complexity to O(n) by using a single array instead of a 2D array.
``` cpp
class Solution {
public:
    
    int lengthOfLIS(vector<int>& nums) 
    {
        int n = nums.size();
        vector<int> prev1(n+1, 0);
        vector<int> temp(n+1,0);
        for(int ind=n-1;ind>=0;ind--)
        {
            for(int prev=ind-1;prev>=-1;prev--)
            {
                int notTake = 0 + prev1[prev+1];
                int take=0;
                if(prev==-1 || nums[ind]>nums[prev])
                {
                    take = 1 + prev1[ind+1];
                }
                temp[prev+1] = max(take, notTake);
            }
            prev1=temp;
        }
        return prev1[0];
    }
};
```
- Time Complexity: O(n^2) (as we are storing the results of subproblem)
- Space Complexity: O(n) (for the dp array) 


### Tabulation
- Here, we find the longest increasing subsequence ending with a particular element.
- We store length of such subsequence in array
- Longest increasing subsequence doesn’t have to end with the last element, it can end before as well.
- lis[i] = It signifies the length of LIS that ends at the index i.

```cpp
int lis(int arr[], int n)
{
    int lis[n];
    lis[0]=1;
    int res=1;
    for(int i=1;i<n;i++)
    {
        lis[i]=1;
        for(int j=0;j<i;j++)
        {
            if(arr[j]<arr[i])
            {
                lis[i]=max(lis[i],lis[j]+1);
            }
        }
        res = max(res, lis[i]);
    }
    
    
    return res;
}
```
- Time Complexity: theta(n^2)
- Space Complexity: theta(n)

- There is a better solution which can be implemented with theta(n*log n) time complexity, using binary search.

### Optimized

- here, we store the minimum possible tail value for every LIS of length (i+1)
- We use binary search to find the ceiling element in the array. i.e., where the element can be inserted in the array.
- Here, if the element in array is grater than the last stored element in tail array, just append it. If it is not, then just find the position where it can be inserted, and replace it with the element already present there.
- Here, in the lis, we are not storing the actual subsequence, but we are storing the tail values of the increasing subsequences. 
```cpp
int ceilInd(vector<int> lis, int s, int e, int x)
{
    while(s<e)
    {
        int m = s + (e-s)/2;
        if(lis[m]>=x)
        {
            e=m;
        }
        else
        {
            s=m+1;
        }
    }
    return s;
}
    int lengthOfLIS(vector<int>& nums) {
        
        vector<int> lis;
        int n = nums.size();
        lis.push_back(nums[0]);
        int len=1;
        for(int i=1;i<n;i++)
        {
            if(nums[i]>lis.back())
            {
                lis.push_back(nums[i]);
                len++;
            }
            else
            {
                // binary search
                int c = ceilInd(lis,0,len-1,nums[i]);
                lis[c]=nums[i];
            }
        }
        return len;
    }
```
- Time Complexity: O(n*log n)
- Space Complexity: theta(n)

## Variations of LIS:
1. Minimum deletions to make array sorted
   - Find LIS
   - Return array length-LIS
2. Maximum sum increasing subsequence
   - Among all the subsequences, we need to find the one with maximum sum.

```cpp
int msum[n];
for(int i=0;i<n;i++)
{
    msum[i]=arr[i];
    for(int j=0;j<i;j++)
    {
        if(arr[j]<arr[i])
        {
            msum[i]=max(msum[i],msum[j]+arr[i]);
        }
    }
}
int res=msum[0];
for(int i=1;i<n;i++)
{
    res=max(res,msum[i]);
}
return res;
```
3. Maximum length of bitonic subsequence
   - Bitonic subsequence = subsequence which is first increasing and then decreasing
   - Increasing or decreasing can also be empty
   - Arrays sorted in increasing or decreasing order can also be considered as bitonic sequence.
   - We will do the same procedure as we did to find the LIS to get the list, but in the reverse direction. We get the longest decreasing subsequence
   - For every pivot I, we find the lis[i]+lds[i]-1, and answer will be the maximum value of the expression for all i’s
   - Time Complexity: O(n^2)
   - Space Complexity: theta(n)
   - We can optimize above solution to O(nlogn) time complexity, using binary search.

4. Building bridges:
   - We are given an array of pair of cities.
   - These pairs represent the requirement for the bridge
   - But the condition is that no 2 bridges should cross each other.
   - We want to maximize the number if bridges we can make.

![alt text](image.png)
   Steps:
   a. Sort the array in increasing order of the first value of pair. If the 2 first values are same, then consider second value.
   b. Find the LIS of the sorted array according to the second value.
   
   - Time Complexity: sorting (O(nlog n)) + LIS (O(nlog n)) = O(nlog n)

5. Longest chain of pairs:
   - We are given array of pairs
   - First element of every pair is smaller than second element of every pair.
   - We need to form the longest chain pf pairs such that suppose if we have 2 pairs (a,b) and (c,d)
   - Then to form the chain b must be smaller than c

   Steps:
   a. Sort the array of pairs according to the first value
   b. Find the LIS of the sorted array

   - This is the acivity selection problem.
   
