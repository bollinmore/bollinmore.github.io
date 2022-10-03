# 374. Guess Number Higher or Lower
[Source](https://leetcode.com/problems/guess-number-higher-or-lower/)  
**Topic**: Binary Search

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API int guess(int num), which returns three possible results:

* -1: Your guess is higher than the number I picked (i.e. num > pick).
* 1: Your guess is lower than the number I picked (i.e. num < pick).
* 0: your guess is equal to the number I picked (i.e. num == pick).

Return the number that I picked.

**Constraints**

* 1 <= n <= 231 - 1
* 1 <= pick <= n

## Concept

The idea is similar to *Binary Search* because we'll move the mid based on what `guess()` return, when:  

* `guess(m) > 0` => set `l = m+1`
* `guess(m) < 0` => set `r = m-1`
* `guess(m) == 0` => Found answer

We could use the condition `while(l<=r)` or `while(true)` because we're ensure to find answer.

## Complexity

**Time Complexity**  

O(logn)

**Space Complexity**  

O(1)

## Special test case

```
2126753390
1702766719
```
The above case will cause buffer overflow exception if our expression is `(r+l)/2`.

## Code
```c++
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is lower than the guess number
 *			      1 if num is higher than the guess number
 *               otherwise return 0
 * int guess(int num);
 */

class Solution {
public:
    int guessNumber(int n) {
        
        int l=0, r=n;
        while (l<=r) {
            int m = (r-l)/2 + l; // Note: (r+l)/2 will cause buffer overflow exception
            int ret = guess(m);
            
            if (ret > 0)
                l = m+1;
            else if (ret < 0)
                r = m-1;
            else
                return m;
        }
        
        return -1;
    }
};
```
