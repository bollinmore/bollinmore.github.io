# 15. 3Sum

[Source](https://leetcode.com/problems/3sum/)  

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set **must not contain duplicate triplets.**

**Constraints**

* 0 <= nums.length <= 3000
* -105 <= nums[i] <= 105

## Concept

**Two Pointers**
Before using *two poiters* strategy, we have to **sort the input array first** so that it can be easily to compare.  
The algorithm is:  

* Sort the array
* Set the `target=0-nums[i]` because we will check the rest elements to find out if there's a pair which sum equal to target.
* The key point is to set left pointer `l` to the next of current digit and set right pointer `r` to the end of array.
  * If `nums[l]+nums[r] < target`, we need to move `l` to the right by 1.
  * If `nums[l]+nums[r] > target`, we need to move `r` to the left by 1.
  * If `nums[l]+nums[r] == target`, it means there's a solution found. Then we iterate moving `l` or `r` pointers if its next elements is equal.

## Complexity

**Time Complexity**  

O(n^2): The sorting takes O(nlogn), and the worst case of visiting elements will be O(n^2). So O(nlogn + n^2) will be O(n^2)

**Space Complexity**  


## Code
```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> vx;
        
        int len = nums.size();
        for (int i=0; i<len-2; i++) {
            if (i>0 && nums[i]==nums[i-1]) continue;
            int l=i+1, r=len-1;
            int target = 0 - nums[i];
            
            while (l<r) {
                
                if (nums[l]+nums[r] < target) l++;
                else if (nums[l]+nums[r] > target) r--;
                else {
                    vx.push_back({nums[i], nums[l], nums[r]});  
                    while (l<r && nums[l]==nums[l+1]) l++;
                    while (l<r && nums[r]==nums[r-1]) r--;
                    l++;
                    r--;
                }
            }
        }
        
        return vx;
    }
};
```
