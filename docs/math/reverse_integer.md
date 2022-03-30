# 7. Reverse Integer

[Source](https://leetcode.com/problems/reverse-integer/)  

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.

Assume the environment does not allow you to store 64-bit integers (signed or unsigned).

**Constraints**

* -231 <= x <= 231 - 1

## Concept

The limitation that you could NOT use a **long** or **long long** to store a temporary data, so we might consider the edge case that could prevent overflow or underflow during checking each digit, the algorithm will be:  

* Detect the input value is negative or not and use a flag to hold it.
* Set the absolute value of `x` to `y` to make the codes easy to implement.
* Check the `y>0` iterately and get remaining `r`.
* Calculate the temporary answer by `v = v*10 +r`, but the key point is:
  * if `v > INT_MAX/10`, return 0
  * if `v == INT_MAX/10 && r > 7`, return 0
* Return the correct answer with previous sign flag.

**Why we need to return 0 in some condition?** It's because any number larger than INT_MAX/10 cannot be multipied by 10 or it would get overflow exception.

## Complexity

**Time Complexity**  

O(log(n)): It's roughly equal log 10 (n).

**Space Complexity**  

O(1)

## Code
```c++
class Solution {
public:
    int reverse(int x) {
        int v=0;
        int sign = x>=0;
        int y = abs(x);
        
        while (y>0) {
            int r = y%10;
            
            if (v > INT_MAX/10) // v >= 214748365
                return 0;
            
            if (v==INT_MAX/10 && r>7) // v >= 214748364 && r > 7
                return 0;
            
            v = v*10 + r;
            y /= 10;
        }
        
        return sign ? v : 0-v;
    }
};
```