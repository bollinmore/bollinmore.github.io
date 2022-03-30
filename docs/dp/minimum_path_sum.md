# 64. Minimum Path Sum

[Source](https://leetcode.com/problems/minimum-path-sum/)  

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

![](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

**Constraints**

* m == grid.length
* n == grid[i].length
* 1 <= m, n <= 200
* 0 <= grid[i][j] <= 100

## Concept

The idea is similar to `Unique Paths` problem, but we use a two dimension array with size m+1 x n+1. The reason that adding one to m & n is to compare value between extended cell with incoming grid. What we do to find out the solution is implement as below:  

* Create a two dimension array `a[m+1][n+1]`
* Initialize the last row and column to all `INT_MAX`. (Here is a common straegy to find out the minimum from two values.)
* Set `a[m][n-1]` to **zero** because we want to **get the smallest value** in the first time.
* Iterate cell starting from the last cell in the grid.
* Find out the smaller value from the two adjacent nodes(right/down) and plus current cell. Repeat this step until we reach the first cell in the grid.
* The final answer is in `a[0][0]`

## Complexity

**Time Complexity**  

O(m*n): We iterate the table, the size of table is m rows multiplies n * columns.

**Space Complexity**  

O(m*n): An extra space to hold the value when calculating the minimum path.

## Code
```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        /** 2022/1/7 -- 8ms, 9.8mb; O(m*n), O(m*n); DP, Copy cat */
        int m = grid.size(), n = grid[0].size();
        
        int rec[m+1][n+1];
        for (int i=0; i<n; i++)
            rec[m][i] = INT_MAX;
        
        for (int i=0; i<m; i++) 
            rec[i][n] = INT_MAX;
        
        rec[m][n-1] = 0; // Either `rec[m][n-1] = 0` or `rec[m-1][n] = 0` could let the min() work.
        
        for (int i=m-1; i>=0; i--) {
            for (int j=n-1; j>=0; j--) {
                rec[i][j] = grid[i][j] + min(rec[i+1][j], rec[i][j+1]);
            }
        }
        
        return rec[0][0];
    }
};
```