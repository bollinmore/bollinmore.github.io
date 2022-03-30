# 142. Linked List Cycle II

[Source](https://leetcode.com/problems/linked-list-cycle-ii/)  

Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.

Do not modify the linked list.


**Constraints**

* The number of the nodes in the list is in the range [0, 104].
* -105 <= Node.val <= 105
* pos is -1 or a valid index in the linked-list.

## Concept:

**O(1) space complexity**  

Since. (2S = F) consider the part from 
start - cycle point = A, 
cycle point- intersect = B, 
intersect - cycle point = C

then slow trival A + B
fast travel A + (B+C) * N + B
(B+C) is one cycle and * N number we travel the cycles

from 2S = F we know (B+C)*N = A + B

from (B+C)*N = A + B if we remove B from each side
B * (N-1) + C * N = A.

then A must be equal to C or eqaul to X * C + Y * B where (X - Y = 1)
In other words A mod(B+C) = C.
`A = B(N-1)+CN =B(N-1)+C(N-1)+C=(B+C)(N-1)+C`  

As a result when we travel from start to point takes A to cycle points
and Intersect to cycle points will be same distance or
travel M cycles then reach to cycle points (from A mod(b+c) = C);
(thats is why we set 1 ptr at start and 1 at intersect and trival at same rate.)
    
## Code:
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;

        while (slow && fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            
            if (slow==fast) {
                ListNode *start = head;
                while (slow != start) {
                    start = start->next;
                    slow = slow->next;
                }
                
                return start;
            }
        }
        
        return nullptr;
    }
};
```