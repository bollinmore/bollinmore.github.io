# 217. Contains Duplicate

[Source](https://leetcode.com/problems/contains-duplicate/)  

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

**Constraints**

* 1 <= nums.length <= 105
* -109 <= nums[i] <= 109

## Concept

There're serveral method could be used to solve this problem - *Sorting*, *map*, *set*.  
**Sorting**  
If we find out a number is equal to its adjacent element, return true immediately.

**map**  
Using number as key of map and the occurence as the value, if there's one occurence more than one, we'll return true.

**set**  
Using *set* as container and copy all nodes to that. Comparing the size between these two containers, return true if they're different.

## Complexity

**Time Complexity**  

* Sorting: O(nlogn)
* Map: O(n)
* Set: (n)

**Space Complexity**  

* Sorting: O(logn), the space needed when sorting elements.
* Map: O(n), we'd use as many as the input number when they're all different.
* Set: (n), same as map.

## Code
```c++
  Sort  
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int len = nums.size();
        for (int i=1; i<len; i++) {
            if (nums[i-1] == nums[i])
                return true;
        }
        
        return false;
    }

  Set
    bool containsDuplicate(vector<int>& nums) {
        set<int> st(nums.begin(), nums.end());
        return nums.size() > st.size();
    }

  Map
    bool containsDuplicate(vector<int>& nums) {
        map<int, int> mp;
        
        for (auto n : nums) {
            mp[n]++;
        }
        
        for (auto m : mp) 
            if (m.second > 1)
                return true;
        
        return false;
    }
```
