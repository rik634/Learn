# Cherry pickup II
- You are given a rows x cols matrix grid representing a field of cherries where grid[i][j] represents the number of cherries that you can collect from the (i, j) cell.

- You have two robots that can collect cherries for you:

1. Robot #1 is located at the top-left corner (0, 0), and
2. Robot #2 is located at the top-right corner (0, cols - 1).

- Return the maximum number of cherries collection using both robots by following the rules below:

1. From a cell (i, j), robots can move to cell (i + 1, j - 1), (i + 1, j), or (i + 1, j + 1).
2. When any robot passes through a cell, It picks up all cherries, and the cell becomes an empty cell.
3. When both robots stay in the same cell, only one takes the cherries.
4. Both robots cannot move outside of the grid at any moment.
5. Both robots should reach the bottom row in grid.
 


### Logic:
- Here,finding the max sum path for both the robots separately will not work, as we might miss out on some of the common elements.
- Here, we need to find the max sum path for both together.
- Here, we have fixed starting point for both the robots. In such case, we write the recursion from the starting point instead of the ending point.
- Also, both the robots will reach the destination or the last row at the same time. This is because, if we observe the steps that are allowed for the robot, at each step, it is going to the next down row, which answers the query.
- As a result, the row index for the co-ordinates of the both robots is same, as considered as only one parameter.



### Base case:
- Base can be either the destination or the out of boundary.

### Steps:
1. Express everything in terms of (i1,j1) and (i2,j2)
2. Explore all the paths (down, down-left, down-right)
3. Max sum

### Recursion
```
int f(i,j1,j2)
{
    if(j1<0 || j1>M ||  j2<0 || j2>M)
    {
        return -1e9;
    }
    if(i==N-1)
    {
        if(j1==j2)
        {
            return A[i][j1];
        }
        else
        {
            return A[i][j1]+A[i][j2];
        }
    }
}
```


### Memoization



### Tabulation



### Space Optimized

