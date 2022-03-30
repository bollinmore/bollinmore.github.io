# 1013. Partition Array Into Three Parts With Equal Sum

[Source](https://leetcode.com/problems/partition-array-into-three-parts-with-equal-sum/)  
**Topic**: Array, Greedy

Given an array of integers arr, return true if we can partition the array into three non-empty parts with equal sums.

Formally, we can partition the array if we can find indexes i + 1 < j with (arr[0] + arr[1] + ... + arr[i] == arr[i + 1] + arr[i + 2] + ... + arr[j - 1] == arr[j] + arr[j + 1] + ... + arr[arr.length - 1])


**Constraints**

* 3 <= arr.length <= 5 * 104
* -104 <= arr[i] <= 104

## Concept

The basic idea is to sum up a range of value and check if it is equal to `total/3`, we'll  

* Increase `part` variable if the condiction meet.
* Finally, return true if `part>=3`, why? Because every single equal to `total/3` also add one to `part`.

## Complexity

**Time Complexity**  

O(n)

**Space Complexity**  

O(1)

## Special test case

* [1,-1,1,-1] => Sum is zero will cause sum % (total/3) get divided by zero exception
* [1,1,1,1]

## Code
```c++
class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& arr) {
        int total = std::accumulate(arr.begin(), arr.end(), 0); //O(n)
        int sz = arr.size();
        int sum = 0, part = 0;
        
        if (total%3 != 0) return false;
            
        for (int i=0; i<sz; i++) {
            sum += arr[i];
            if (sum == total/3) {
                part++;
                sum=0;
            }
        }
        
        return part >= 3; // '>=' is used to filter out a single value which satisfied `sum==total/3` that will increase the value of part
    }
};
```
