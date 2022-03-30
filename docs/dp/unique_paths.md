# 62. Unique Paths

[Source](https://leetcode.com/problems/unique-paths/)  

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]).   
The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]).   
The robot can only move either down or right at any point in time.  
Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.  
The test cases are generated so that the answer will be less than or equal to 2 * 109.

**Constraints**

* 1 <= m, n <= 100

## Concept:

|   |   |   |   |   |   |   | 
|---|---|---|---|---|---|---| 
| 28 | 21 | 15 | 10 | 6  | 3  | 1  | 
| 7  | 6  | 5  | 4  | 3  | 2  | 1  | 
| 1  | 1  | 1  | 1  | 1  | 1  | 1  | 

The idea is is to **solve sub problem** start from the bottom-right:  

* Create a 2D array to hold every possible path to the desitination.
* Set the value of bottom row to all **1** because there's only one way to each cell.
* Sum up the value in right & bottom of each cell => It shows how much ways to the destination starting from current cell.
* We'd get final answer at `grid[0][0]`

## Complexity

**Time Complexity**  

O(m*n): We iterate the table, the size of table is m rows multiplies n * columns.

**Space Complexity**  

O(n): We use a vector to hold the date of previous row, and update it when we finish iterating current row.

## Code:
```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        /** 2022/1/6 -- 7ms, 6.3mb; O(m*n), O(n); DP */
        vector<int> vx(n, 1); // n * 1
        for (int i=0; i<m-1; i++) { // Because we've set the bottom row to all 1
            vector<int> vNew(n, 1); // Since we want the last element become 1
            for (int j=n-2; j>=0; j--) { // Because the last column is always 1
                vNew[j] = vNew[j+1] + vx[j];
            }
            
            vx.assign(vNew.begin(), vNew.end());
        }
        
        return vx[0];
    }
};
```