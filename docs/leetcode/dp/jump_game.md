# Jump Game

[Source](https://leetcode.com/problems/jump-game)  

You are given an integer array nums.  
You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

**Constraints**

* 1 <= nums.length <= 104
* 0 <= nums[i] <= 105

## Concept

The idea is to iterate entire loop and set variable `reach` by **max(reach, i+nums[i])**, when the indexer `i` exceed `reach` which means we couldn't jump to the end with current value.  
    
## Code
```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        /** 2022/1/6 -- O(N), O(1); Greedy */
        int n = nums.size();
        if (n == 1) return true;
        
        int i = 0;
        for (int reach=0; i<n && i<=reach; i++) { // We add `i<=reach` to let i escape from loop earlier
            reach = max(reach, i+nums[i]);
        }
        
        return i == n;
    }
};
```