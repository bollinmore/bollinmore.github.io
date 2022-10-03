# 219. Contains Duplicate II

[Source](https://leetcode.com/problems/contains-duplicate-ii/)  
**Topic**: Hash Table, Sliding Window

Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k.

**Constraints**

* 1 <= nums.length <= 105
* -109 <= nums[i] <= 109
* 0 <= k <= 105

## Concept
**Map**  
Unlikely to #217, we set the *offset* as the value rather than occurence, so we can add or update the offset if the key is not exist.  
Otherwise, we can calculate the difference from current index to `mp[nums[i]]`, and return true if it's less than `k`.

**Set**  
Using *set* as a container to simulate the sliding window because we only care about those value index `i` to `i-k+1`, so the key point is to **erase the out bound value**  
by `st.erase(nums[i-k-1])`. The reason to specify a value with offset `i-k-1` is because the *insert* operation will put new value at the front of container, so we can't erase the first.  

## Complexity

**Time Complexity**  

* Map: O(n)
* Set: O(n), either `st.erase` & outer loop may take linear time complexity.

**Space Complexity**  

* Map: O(n)
* Set: O(n)

## Special test case


## Code
```c++
  Map
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        map<int, int> mp;
        int len = nums.size();
        
        for (int i=0; i<len; i++) {
            int v = nums[i];
            if (!mp.count(v) || i-mp[v]>k)
                mp[nums[i]] = i;
            else 
                return true;
        }
        
        return false;
    }

  Set
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int len = nums.size();
        unordered_set<int> st;
        
        for (int i=0; i<len; i++) {
            if (i>k) st.erase(nums[i-k-1]);
            if (st.find(nums[i]) != st.end()) return true;
            st.insert(nums[i]);
        }
        
        return false;
    }
```
