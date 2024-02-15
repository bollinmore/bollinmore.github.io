# 141. Single Number

[Source](https://leetcode.com/problems/linked-list-cycle/)  
**Topic**: Array, Prefix Sum

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

True:  
![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

False:  
![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)


**Constraints**

* The number of the nodes in the list is in the range [0, 104].
* -105 <= Node.val <= 105
* pos is -1 or a valid index in the linked-list.

## Concept

Utilize a set data structure to store visited nodes. If any node is found in that set, it indicates the presence of a cycle in the graph.

## Complexity

**Time Complexity**  

O(n)
* Iterate through the linked list to determine if it reaches the end or if a node has been visited.

**Space Complexity**  

O(n)

## Special test case


## Code
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
    bool hasCycle(ListNode *head) {
        ListNode *curr = head;
        std::set<ListNode*> visited;

        while (true) {
            if (curr == nullptr)
                return false;

            if (visited.find(curr) == visited.end())
                visited.insert(curr);
            else
                return true;

            curr = curr->next;
        }
    }
};
```
