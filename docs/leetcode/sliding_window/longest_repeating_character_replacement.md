# 424. Longest Repeating Character Replacement

[Source](https://leetcode.com/problems/longest-repeating-character-replacement/)  
**Topic**: Sliding Window, Hash Table, String

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

**Constraints**

* 1 <= s.length <= 105
* s consists of only uppercase English letters.
* 0 <= k <= s.length

## Concept

**Sliding Window**  
We use `l` & `r` as the bound of a window, the main idea is to check if *Window_Size - the number of majority <= k*, why?  
Since there's only `k` chances that we can replace characters, so a valid window will have the number which less than k for replacement.  
How do we find the maxmimum lenght of a repeating string? Use a variable `res` to keep track of the max value and update the result only when a valid window size is more than `res`.

## Complexity

**Time Complexity**  

O(n): n is equal to string length.

**Space Complexity**  

O(1): the worst case will be n=26 becasue there're 26 distinct characters in the string.

## Special test case


## Code
```c++
class Solution {
public:
    static bool compare(const pair<char, int>& a, const pair<char, int>& b){
        return a.second <= b.second;
    }
    
    int characterReplacement(string s, int k) {
        int l=0, r=0, res=0;
        map<char, int> m; // key: character, value: frequency
        int maxf=0;
        
        for (auto c : s) {
            int winLen = r-l+1;
            m[c]++;
            maxf = max(maxf, m[c]);
            
            // while (winLen - max_element(m.begin(), m.end(), compare)->second > k) {
            while (winLen - maxf > k) { // Use maxf instead of searching from map
                m[s[l]]--;
                l++;
                winLen = r-l+1;
            }
            
            res = max(res, winLen);
            r++;
        }
        
        return res;
    }
};
```
