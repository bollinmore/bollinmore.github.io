# 134. Gas Station

[Source](https://leetcode.com/problems/gas-station/)  

There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique

**Constraints**

* gas.length == n
* cost.length == n
* 1 <= n <= 105
* 0 <= gas[i], cost[i] <= 104

## Concept:

**One pass algorithm**  
The basic idea is:

* The total gas must larger than total cost, otherwise there is no way to travel around the circuit.
* If the tank is less **zero**, we move the starting point to the next station.
* Why one pass works is because we assuming the oil is sufficient so we can always reset the starting point if `tank + gas - cost < 0`

## Complexity

**Time Complexity**  

O(n^2): We have a nested for-loop inside because we need to move root node.

**Space Complexity**  

O(n): Extra space to hold the number of unique BST of N.

## Code:
```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        /** 2022/1/21 -- O(n), O(1); One pass */
        int tank = 0;
        int start = 0, total_gas = 0, total_cost = 0;
        
        for (int i=0; i<gas.size(); i++) {
            total_gas += gas[i];
            total_cost += cost[i];
            tank += (gas[i] - cost[i]);
            
            if (tank < 0) { // 
                start = i+1;
                tank = 0;
            }
        }
        
        return (total_gas >= total_cost) ? start : -1;
    }
};
```