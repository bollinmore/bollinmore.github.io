# 96. Unique Binary Search Trees

[Source](https://leetcode.com/problems/unique-binary-search-trees/)  

Given an integer n, return the number of structurally unique BST's (binary search trees) which has exactly n nodes of unique values from 1 to n.

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

**Constraints**

* 1 <= n <= 19

## Concept:

The idea is to **solve sub problem** in dynamic programming.  
When `n = 0` or `n = 1`, there's 1 possible BST exists. But when the `n >= 2` that means we could break down this problem into sub problem as below:  

* n==2: **left nodes + root + right nodes**
```
   2       1
  /   or    \
 1           2
```

So we can turn this problem into **Find out the combination** while iterate the nodes, the formula of each combination will be `numTrees(left) * numTrees(right)`, then we can sum up the number because we want to know how many unique BST exists for N.

## Complexity

**Time Complexity**  

O(n^2): We have a nested for-loop inside because we need to move root node.

**Space Complexity**  

O(n): Extra space to hold the number of unique BST of N.

## Code:
```c++
class Solution {
public:
    int numTrees(int n) {
        if (n<=1) return 1;
        vector<int> dp(n+1, 1); // 0-> 1; 1->1
        
        for (int nodes=2; nodes<=n; nodes++) {
            int total = 0;
            for (int root=1; root<=nodes; root++) {
                int left = root - 1;
                int right = nodes - root;
                total += dp[left] * dp[right];
            }
            dp[nodes] = total;
        }
        
        return dp.back();
    }
};
```