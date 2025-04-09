# DP
- It is simply the optimization over plane recursion.
- Idea is to reuse the solutions of the subproblem when there are overlapping subproblem.
    1. Memoization (top down)
       - It tends to store the value of the subproblem in map or table
    2. tabulation (bottom up)
       - 

- Dimension of the memo array depends upon the number of parameters we are passing to the function, and are changing in the recursive calls. And size depends upon max and min value of parameters.
- Here, the number of parameters we are passing is 1, so, memo array is 1-dimensional. While, if we are passing 2 parameters, memo array is 2-dimensional.
- For initializing the memo array, assign such a value which is not a possible answer.

## Steps to follow:
1. First write the recursive solution
2. Find out about the overlapping subproblem, then we find it’s a DP problem.

### Steps for Recursion to memoization
1. Declare DP array considering the size of the subproblem
2. Check in DP array, if the value was previously computed. If yes, just return it. Otherwise go for computing the answer for it.
3. Storing the answer in array for every subproblem computed and return it.


## Applications:
1.	Bellman ford algorithm (used to find the shortest path from source to destination)
2.	Floyd warshall algorithm (used to find the shortest path between every pair of vertices)
3.	Diff utility (LCS-Longest common subsequence) (to find difference between 2 files)
4.	Search closed words (Edit distance) (searching a word in dictionary which is closest to the word u are searching for)
5.	Resource allocation (0-1 knapsack, suppose we have some requirements of resources and we have some options, we can make optimized decisions using this)


## Questions:
1. Fibonacci Problem
2. Longest Common Subsequence
3. Coin Change
4. Edit Distance
5. Longest Increasing Subsequence
6. Maximum cuts
7. Minimum coins to make a value
8. Minimum jumps to reach end
9. 0-1 Knapsack Problem
10. Optimal strategy for a game
11. Egg dropping Problem
12. Count BSTs with n keys
13. Maximum sun with no consecutive
14. Subset sum problem
15. Matrix chain multiplication
16. Palindrome partitioning
17. Allocate minimum number of pages



