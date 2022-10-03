# 1. Two Sum

[Source](https://leetcode.com/problems/two-sum/)  

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?

**Constraints**

* 2 <= nums.length <= 104
* -109 <= nums[i] <= 109
* -109 <= target <= 109
* Only one valid answer exists.

## Concept

Because the limitation is to implement a algorithm which time complexity is less O(n^2), so we use an extra space to keep track of indice in a map.  
Then we iterately check if the complement exists in the map.

## Complexity

**Time Complexity**  

O(n): We only visit each element one time in the worst case.

**Space Complexity**  

O(n): We use an extra space to map the complement and index.

## Code
```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> m;
        for (int i=0; i<nums.size(); i++) {
            int diff = target - nums[i];
            if (m.count(diff) > 0)
                return {m[diff], i};
            
            m[nums[i]] = i;
        }
        
        return {};
    }
}; 
```
