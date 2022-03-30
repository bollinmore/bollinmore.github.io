# 414. Third Maximum Number

[Source](https://leetcode.com/problems/third-maximum-number/)  

Given an integer array nums, return the third distinct maximum number in this array. If the third maximum does not exist, return the maximum number.

**Follow up: Can you find an O(n) solution?**

**Constraints**

* 1 <= nums.length <= 104
* -231 <= nums[i] <= 231 - 1

## Concept

The idea is to find out the top three numbers in one pass algorithm, but we need to take care of the 3rd largest number might **INT_MIN**.  
So we use `long long` as the data type of a vector, and intialize its data to `LLONG_MIN`.  

* Create a vector `vector<long long> vx(3, LLONG_MIN)`
* Iterately compare the number between `nums` and `vx`
* We need to keep the order inside `vx` if we find a number is larger than any digit in `vx`
* Since we initialized data to LLONG_MIN previosuly, so we can return `vx[0]`(max) if `vx[2]` is LLONG_MIN which means there's no 3rd biggest number exist.

## Complexity

**Time Complexity**  

O(n)

**Space Complexity**  

O(1)

## Code
```c++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        vector<long long> vx(3, LLONG_MIN);
        
        for (int i=0; i<nums.size(); i++) {
            if (find(vx.begin(), vx.end(), nums[i]) != vx.end())
                continue;
            
            if (nums[i] > vx[0])
                vx = {nums[i], vx[0], vx[1]};
            else if (nums[i] > vx[1])
                vx = {vx[0], nums[i], vx[1]};
            else if (nums[i] > vx[2])
                vx = {vx[0], vx[1], nums[i]};
        }
        
        return vx[2]==LLONG_MIN ? vx[0] : vx[2];
    }
};
```
