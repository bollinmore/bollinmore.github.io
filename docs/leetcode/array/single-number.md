# 136. Single Number

[Source](https://leetcode.com/problems/single-number/)  
**Topic**: Array, Prefix Sum

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.
You must implement a solution with a linear runtime complexity and use only constant extra space.

**Constraints**

* 1 <= nums.length <= 3 * 104
* -3 * 104 <= nums[i] <= 3 * 104
* Each element in the array appears twice except for one element which appears only once.

## Concept

Utilize a map data structure to store the occurrences of each number. The solution to this problem involves finding the key where its corresponding value is equal to one.

## Complexity

**Time Complexity**  

O(n)
* Iterate through the numbers to count their occurrences.
* Search the map to identify the key where the value is equal to one.

**Space Complexity**  

O(n)

## Special test case


## Code
```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        std:map<int, int> m;
        for (auto n : nums) {
            m[n] = (m.find(n) == m.end()) ? 1 : m[n]+1;
        }

        int ans;
        for (std::pair<const int,int>& x : m) {
            if (x.second == 1)
                ans = x.first;
        }

        return ans;
    }
};
```
