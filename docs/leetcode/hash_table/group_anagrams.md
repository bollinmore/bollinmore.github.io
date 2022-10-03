# 49. Group Anagrams

[Source](https://leetcode.com/problems/group-anagrams/)  

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Constraints**

* 1 <= strs.length <= 104
* 0 <= strs[i].length <= 100
* strs[i] consists of lowercase English letters.

## Concept

**Sorting**  
The idea is to sort incoming string and set as a key in the map. Once we find out a key exists, then append the orginal string to the value.

## Complexity

**Time Complexity**  

O(n * klogk): n is the lenght of `strs` and k is the string size. 

**Space Complexity**  

O(n * k)

## Code
```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> ans;
        unordered_map<string, vector<string>> m;
        
        for (auto s : strs) {
            string s1 = s;
            sort(s1.begin(), s1.end());
            m[s1].push_back(s);
        }
        
        for (auto x : m) {
            ans.push_back(x.second);
        }
        
        return ans;
    }
};
```
