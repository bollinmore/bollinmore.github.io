# 56. Merge Intervals

[Source](https://leetcode.com/problems/merge-intervals/)  

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Constraints**

* 1 <= intervals.length <= 104
* intervals[i].length == 2
* 0 <= starti <= endi <= 104

## Concept

If the source array has been sorted, then we could compare the second value of last item in the output list in one pass.  
So what is **overlap** in this problem? We could define the overlap as `intervals[i-1][1] < intervals[i][0]`.  
Since the intervals is sorted previously which means we could put the first item to output array and compare next item with output array one by one.

## Complexity

**Time Complexity**  

O(nlogn): Assuming the sorting algorithm is O(nlogn). Then the following one pass check(*linear scan*) has complexity O(n)

**Space Complexity**  

O(logn): The sorting algorithm might need an extra space.

## Special test case
* [[1,4],[0,0]]
* [[2,3],[4,5],[6,7],[8,9],[1,10]]

## Code
```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> merged;
        sort(intervals.begin(), intervals.end()); // Sort by the first element, so we can easily compare the second in one pass
        
        for (auto val : intervals) {
            if (merged.empty() || merged.back()[1] < val[0]) // Append to list if it's not overlapped.
                merged.push_back(val);
            else
                merged.back()[1] = max(merged.back()[1], val[1]); // If it's overlap, pick max from the two tails.
        }
        
        return merged;
    }
};
```
