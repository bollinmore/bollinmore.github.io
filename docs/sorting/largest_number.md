# 179. Largest Number

[Source](https://leetcode.com/problems/largest-number/)  

Given a list of non-negative integers nums, arrange them such that they form the largest number and return it.

Since the result may be very large, so you need to return a string instead of an integer.

**Constraints**

* 1 <= nums.length <= 100
* 0 <= nums[i] <= 109

## Concept

We need to *customize a sorting comparator* in our way. Before to sort the array, we have to convert the data type from integer to string to prevent from overflow during sorting.  
Consider a case of `[21474836, 7]`, we would get an overflow exception while compose **7 + 21474836** because it's bigger than INT_MAX.  

The rule of comparator is `s1+s2 > s2+s1` where s1 and s2 is the string in the array.  

After sorting is complete, we should check the resultant string by removing the leading zero because the edge case `[0,0,0]` could come out with `000`.  

## Complexity

**Time Complexity**  

O(nlogn): The sorting algorithm.

**Space Complexity**  

O(n): Need an extra space to store the result.

## Special test case
* [0,0,0,0,0,0,0]
* [1,2,3,4,5,6,7,8,9,0]

## Code
```c++
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        vector<string> strs;
        std::transform(nums.begin(), nums.end(), std::back_inserter(strs), [](int n) {return std::to_string(n); });
        sort(strs.begin(), strs.end(), [](string &s1, string &s2){ return s1+s2>s2+s1; });
        
        stringstream ss;
        for (auto s : strs) {
            ss << s;
        }
        
        string ans = ss.str();
        while (ans[0]=='0' && ans.length()>1)
            ans.erase(0, 1);
        
        return ans;
    }
};
```
