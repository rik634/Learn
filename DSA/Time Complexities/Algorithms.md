# Time Complexities of Algorithms:


## Rough:

1. Bellman Ford (uses graph, DP is used here): used to find the shortest path from source to destination
2. Floyd warshall algorithm: used to find the shortest path between every pair of vertices






## Application:
### DP
1.	Bellman ford algorithm (used to find the shortest path from source to destination)
2.	Floyd warshall algorithm (used to find the shortest path between every pair of vertices)
3.	Diff utility (LCS-Longest common subsequence) (to find difference between 2 files)
4.	Search closed words (Edit distance) (searching a word in dictionary which is closest to the word u are searching for)
5.	Resource allocation (0-1 knapsack, suppose we have some requirements of resources and we have some options, we can make optimized decisions using this)


### Moore Voting Algorithm

- The intuition behind the Moore's Voting Algorithm is based on the fact that if there is a majority element in an array, it will always remain in the lead, even after encountering other elements.

- Questions: Given an array nums of size n, return the majority element.
The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

Explanation - Algorithm:
- Initialize two variables: count and candidate. Set count to 0 and candidate to an arbitrary value.
- Iterate through the array nums:
a. If count is 0, assign the current element as the new candidate and increment count by 1.
b. If the current element is the same as the candidate, increment count by 1.
c. If the current element is different from the candidate, decrement count by 1.
After the iteration, the candidate variable will hold the majority element.
```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        
        int n = nums.size();
        int count=0;
        int candidate=0;
        for(int i=0;i<n;i++)
        {
            if(count==0)
            {
                candidate=nums[i];
                count++;
            }
            else if(candidate==nums[i])
            {
                count++;
            }
            else
            {
                count--;
            }
        }
        return candidate;
    }
};
```