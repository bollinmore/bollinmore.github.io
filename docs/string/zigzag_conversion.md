# 6. Zigzag Conversion

[Source](https://leetcode.com/problems/zigzag-conversion/)  
**Topic**: 

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);


**Constraints**

* 1 <= s.length <= 1000
* s consists of English letters (lower-case and upper-case), ',' and '.'.
* 1 <= numRows <= 1000

## Concept

Image we have multiple rows and then we put character one by one to each row, what we do is:

* Make sure we have enough rows to put character
* Change the direction if the index reach to the bound

## Complexity

**Time Complexity**  

O(n): n == s.length()

**Space Complexity** 

O(n): n is max(numRows, s.length())

## Special test case

We add `max(numRows, s.length())` when initializing vector to make sure there will have sufficient space. 
```
"AB"
1
```

## Code
```c++
class Solution {
public:
    string convert(string s, int numRows) {
        vector<string> vx(max(numRows, (int)s.length()), "");
        int idx = 0;
        bool godown = true;
        
        for (char c : s) {
            vx[idx].push_back(c);
            idx += godown ? 1 : -1;
            
            if (idx==0 || idx==numRows-1) godown = !godown;
        }
        
        string ans;
        for (string str : vx) {
            ans += str;
        }
        
        return ans;
    }
};
```
