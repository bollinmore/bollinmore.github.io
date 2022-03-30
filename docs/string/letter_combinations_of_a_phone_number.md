# 17. Letter Combinations of a Phone Number

[Source](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)  
**Topic**: 

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Constraints**

* 0 <= digits.length <= 4
* digits[i] is a digit in the range ['2', '9'].

## Concept

We need a vector to store the letter, and the output array will be initialized to ""(`vector<string> out(1, "")`). The answer comes after the following steps:  

1. Iterate the input string `digits`
2. For every digit which we are visiting, then enumerate letters regarding current digit.
3. Combine the string separately with current letter.
4. Using `assign()` or `swap()` to change the content of output buffer. 

## Complexity

**Time Complexity**  

O(4^n): n is the length of digits

**Space Complexity**  

## Special test case


## Code
```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return {};
        vector<string> ss = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> out(1, ""); // Key point, need to initialze as ""
        
        for (auto d : digits) {
            vector<string> vx;
            for (auto c : ss[d - '0']) { // Relative offset to '0'
                for (auto o : out) { // Because we've initialized to "", so there will "" + "a"...
                    vx.push_back(o+c);
                }
            }
            // out.assign(vx.begin(), vx.end());
            out.swap(vx); // This should be constant time.
        }
        
        return out;
    }
};
```
