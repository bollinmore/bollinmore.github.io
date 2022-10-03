# 2. Add Two Numbers

[Source](https://leetcode.com/problems/add-two-numbers/)  

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. **Add the two numbers and return the sum as a linked list.**

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

![](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

**Constraints**

* The number of nodes in each linked list is in the range [1, 100].
* 0 <= Node.val <= 9
* It is guaranteed that the list represents a number that does not have leading zeros.

## Concept:

Algorithm:  

* Create a dummy node and a pointer `curr` which point to the dummy node.
* Create a variable `carry` as a flag to know if there's a carry bit been set.
* Visit l1 & l2 iterately, sum up l1 or l2 if it's not null. **We need to add `carry` if it present'**
* Update the carry flag by set `carry=sum/10` every time.
* Create a new node with value `tmp%10` and let `curr` point to that address.
* Move pointer `curr` to the next by `curr = curr->next`
* Repeart until l1 & l2 are visited both.
* Remember to create a new node if the carry flag is set.

## Complexity

**Time Complexity**  

O(n): Where n is max(l1.length, l2.length).

**Space Complexity**  

O(n): Where n is max(l1.length, l2.length).

## Code:
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *p=l1, *q=l2, *dummy, *curr;
        int carry=0;
        
        dummy = new ListNode();
        curr = dummy; // a pointer to the dummy node
        
        while (p || q) {
            int tmp = 0 + carry;
            if (p) tmp += p->val;
            if (q) tmp += q->val;
            
            carry = tmp/10;
            curr->next = new ListNode(tmp%10); // make link to the dummy
            curr = curr->next; // move curr pointer to next one
            
            if (p) p = p->next;
            if (q) q = q->next;
        }
        
        if (carry) // ethier 0 or 1
            curr->next = new ListNode(carry);
        
        return dummy->next;
    }
};
```