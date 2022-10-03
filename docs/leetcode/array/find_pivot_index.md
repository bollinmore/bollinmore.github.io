# Find Pivot Index

[Source](https://leetcode.com/problems/find-pivot-index/)  
**Topic**: Array, Prefix Sum

Given an array of integers nums, calculate the pivot index of this array.

The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right.

If the index is on the left edge of the array, then the left sum is 0 because there are no elements to the left. This also applies to the right edge of the array.

Return the leftmost pivot index. If no such index exists, return -1.


**Constraints**

* 1 <= nums.length <= 104
* -1000 <= nums[i] <= 1000

## Concept

Since we know the sum of left part is equal to the right part, and the *sum of all elements* will not change, we could solve this problem by **checking the left part and right part iteratively**.  
How do we get the sum of right part? You'll get the sum by `total - leftSum - pivot`.

## Complexity

**Time Complexity**  

O(n): O(n) for getting sum of all numbers; O(n) for checking the leftSum & rightSum.

**Space Complexity**  

O(1)

## Special test case


## Code
```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int sz = nums.size();
        int leftSum=0, total=0;
        
        total = std::accumulate(nums.begin(), nums.end(), 0);  //O(n)
        
        for (int i=0; i<sz; i++) {
            int rightSum = total - nums[i] - leftSum;
                
            if (leftSum == rightSum)
                return i;
                
            leftSum += nums[i];
        }
        
        return -1; // not found
    }
};
```
