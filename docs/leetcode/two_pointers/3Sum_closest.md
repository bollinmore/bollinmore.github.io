# 16. 3Sum Closest

[Source](https://leetcode.com/problems/3sum-closest/)  

Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

**Constraints**

* 3 <= nums.length <= 1000
* -1000 <= nums[i] <= 1000
* -104 <= target <= 104

## Concept

This problem is similar to *3 Sum*, so we could solve it with *Two Pointers* strategy, the algorithm will be:  

* Sort the input array so that we will adjust the sum by moving left/right pointers.
* Initialize variable `cloeset` as the sum of first 3 values because we know there will be at least 3 elements in the array.
* Note that we can't set `cloeset = INT_MAX` because it'd raise overflow exception when the target is negative during the statement `abs(closest-target)`.
* Then we iterately compare the sum with target by moving left/right pointers.
* Re-assign closest to sum when we got a smaller answer exist.

## Complexity

**Time Complexity**  

O(nlogn + nlogn) = O(nlogn): Where the outer loop has complexity O(n) and inner loop has complexity (logn). The sorting has complexity O(nlogn)

**Space Complexity**  

O(1): Constant memory.

## Code
```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int closest = nums[0] + nums[1] + nums[2]; // nums.size() >= 3 && cannot be INT_MAX since abs(closest-target) may cause overflow with case [1,1,1,1] -100
        int len = nums.size();
        
        for (int i=0; i<len-2; i++) {
            int l = i+1, r=len-1;
            
            while (l<r) {
                int sum = nums[i]+nums[l]+nums[r];
                
                if (sum > target) r--;
                else l++;    
                
                if (abs(sum-target) < abs(closest-target))
                closest = sum;
            }
        }
        
        return closest;
    }
};
```
