# 292. Nim Game

[Source](https://leetcode.com/problems/nim-game/)  

You are playing the following Nim Game with your friend:

Initially, there is a heap of stones on the table.
You and your friend will alternate taking turns, and you go first.
On each turn, the person whose turn it is will remove 1 to 3 stones from the heap.
The one who removes the last stone is the winner.
Given n, the number of stones in the heap, return true if you can win the game assuming both you and your friend play optimally, otherwise return false.

**Constraints**

* 1 <= n <= 231 - 1

## Concept

Because you could take either 1 or 2 or 3 stones each time, so you win the game when the total number of stones less than 4.  
Why does this matter? Consider the case that there are 5-7 stones on the table, and we could pick 1 / 2 / 3 respectively to let the number of the rest stones to 4, 
but you could NOT win when the number is 4 or 8, so we could assume that the number is not divisible by 4 is the answer.  

## Complexity

**Time Complexity**  

O(1)

**Space Complexity**  

O(1)

## Code
```c++
class Solution {
public:
    bool canWinNim(int n) {
        return n&3; // or return n%4 != 0;
    }
};
```
