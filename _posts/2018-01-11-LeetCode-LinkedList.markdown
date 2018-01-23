---
layout:     post
title:      "LeetCode: Top Interview Questions - Linked List"
subtitle:   "这里是Linked List类型题的锦集"
date:       2018-01-11 12:00:00
author:     "YuXiao"
header-img: "img/post-bg-miui6.jpg"
header-mask: 0.3
catalog:    true
tags:
    - LeetCode
    - Top Interview Questions
    - LeetCode-LinkedList
---

#### 21. Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4

```cpp
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
  ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {

    // create two head pointer to keep track of l1 & l2
    ListNode* h1 = new ListNode(0);
    ListNode* h2 = new ListNode(0);
    h1->next = l1;
    h2->next = l2;

    // create solution pointer
    ListNode* h0 = new ListNode(0);
    ListNode* t0 = h0;

    // attaching node to h0 from l1, l2 acorrding to order
    while (h1->next && h2->next){
      if (h1->next->val < h2->next->val){
        t0->next = h1->next;
        t0 = t0->next;
        h1->next = h1->next->next;
      }
      else {
        t0->next = h2->next;
        t0 = t0->next;
        h2->next = h2->next->next;
      }
    }

    // attaching rest of the list from l1 or l2
    if (h1->next){
      t0->next = h1->next;
    }
    else {
      t0->next = h2->next;
    }
    return h0->next;
  }
};
```
---

#### 237. Delete Node in a Linked List

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

```cpp
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
  void deleteNode(ListNode* node) {
    if (node->next){
      node->val = node->next->val;
      node->next = node->next->next;
    }
  }
};
```

#### 141. Linked List Cycle

Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

**Thought Process:**
This a classic question that can be solved by fast and slow pointers.
Create two pointers, one move slow jump one node a time, one move fast jump two node a time.
If there is a cycle in this linked list, then the pointers will eventually meet each other.

```cpp
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
    if (!head) return false;
    ListNode* slow = head;
    ListNode* fast = head->next;
    while (slow && fast){
      if (slow == fast){
        return true;
      }
      if (!fast->next) break;
      slow = slow->next;
      fast = fast->next->next;
    }
    return false;
  }
};
```

#### 234. Palindrome Linked List

Given a singly linked list, determine if it is a palindrome.

Follow up:
Could you do it in O(n) time and O(1) space?

**Thought Process:**
We have two approaches for this question.

*Approach 1: O(n) runtime, O(n) space.*
In order to check if the linked list is a palindrome, we need to find the middle node of the linked list. It is very easy to do with array since we know the size and we can have instant access to any position in an array. But for linked list, it is not that easy, to find the middle node, we can use fast, slow pointers, fast pointer move two node a time, slow pointer move one node a time. Once fast pointer have reach the end, the slow pointer will reach the middle node. To check if for palindrome we can use a `stack` to store the node's value when moving slow pointer. After the slow pointer have reached the middle point, we have all the value that appeared in the first half of the linked list in the `stack`, then we can utilize the feature that `stack` provide, "first in last out", to check if the second half of the linked list have the same value but in reversed order.

*Approach 2: O(n) runtime, O(1) space.*
The follow up question is asking us to solve the problem using O(n) time and O(1) space. With this condition, we cannot use `stack` anymore. However, we can still use the "fast, slow pointers" technique to find the middle node. After that, we can use few additional pointers to reverse the second half of the linked list. Finally, we can perform the check by compare the first half of the linked list to the second half where it was reversed.

```cpp
/**
* Definition for singly-linked list.
* struct ListNode {
*     int val;
*     ListNode *next;
*     ListNode(int x) : val(x), next(NULL) {}
* };
*/

// Approach with O(n) runtime and O(n) space
class Solution {
public:
  bool isPalindrome(ListNode* head) {
    if (!head || !head->next) return true;
    ListNode* slow = head;
    ListNode* fast = head;
    stack<int> s;
    s.push(head->val);
    while (fast->next && fast->next->next){
      slow = slow->next;
      fast = fast->next->next;
      s.push(slow->val);
    }
    if (!fast->next) s.pop();
    while (slow->next){
      slow = slow->next;
      if (s.top() != slow->val){
        return false;
      }
      s.pop();
    }
    return true;
  }
};

// Approach with O(n) runtime and O(1) space
class Solution {
public:
  bool isPalindrome(ListNode* head) {
    if (!head || !head->next) return true;
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast->next && fast->next->next){
      slow = slow->next;
      fast = fast->next->next;
    }
    ListNode* last = slow->next;
    ListNode* pre = head;
    while (last->next){
      ListNode* temp = last->next;
      last->next = temp->next;
      temp->next = slow->next;
      slow->next = temp;
    }
    while (slow->next) {
      slow = slow->next;
      if (pre->val != slow->val) return false;
      pre = pre->next;
    }
    return true;
  }
};
```

#### 160. Intersection of Two Linked Lists
Write a program to find the node at which the intersection of two singly linked lists begins.


For example, the following two linked lists:
```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗
B:     b1 → b2 → b3
```
begin to intersect at node c1.


Notes:

If the two linked lists have no intersection at all, return null.
The linked lists must retain their original structure after the function returns.
You may assume there are no cycles anywhere in the entire linked structure.
Your code should preferably run in O(n) time and use only O(1) memory.

**Thought Process:**
If the length of both linked list is the same, this question will very easy, we can just check if nodes in both linked list one by one and see if there is a point they intersect. However, we need to consider the case that they may have different sizes, so we need to find the length of both linked lists, then we traverse the longer one with their length difference, then check the node one by one by their difference.

```cpp
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
   ListNode* getIntersectionNode(ListNode *headA, ListNode *headB) {
     if (headA == NULL || headB == NULL){
       return NULL;
     }

     ListNode* pA = headA;
     ListNode* pB = headB;

     int lenA = 0, lenB = 0;

     while (pA || pB){
       if (pA){
         lenA += 1;
         pA = pA->next;
       }
       if (pB){
         lenB += 1;
         pB = pB->next;
       }
     }

     pA = headA;
     pB = headB;

     if (lenA > lenB){
       int diff = lenA - lenB;
       while (diff > 0){
         pA = pA->next;
         diff -= 1;
       }
     }
     else {
       int diff = lenB - lenA;
       while (diff > 0){
         pB = pB->next;
         diff -= 1;
       }
     }

     while (pA){
       if (pA == pB){
         return pA;
       }
       pA = pA->next;
       pB = pB->next;
     }
     return NULL;
   }
 };
```

#### 2. Add Two Numbers
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

**Thought Process:**
The first approach I tried is first convert the two linked list into integers, get the sum of them, then convert the sum back to a linked list as required. However, this approach failed a test case where the input linked list is very long, then there was a integer size overflow problem.

The second approach is shorter and better, where it does not raise the overflow problem. We use a variable `carry` store the calculated values of each digits(nodes) in both linked lists. Then input it to the digits of the result linked list.

```cpp
/**
* Definition for singly-linked list.
* struct ListNode {
*     int val;
*     ListNode *next;
*     ListNode(int x) : val(x), next(NULL) {}
* };
*/

// First Approach, this approach have the problem of integer overflow
class Solution {
public:
  ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode* p1 = l1;
    ListNode* p2 = l2;

    int n1 = p1->val, n2 = p2->val;
    p1 = p1->next;
    p2 = p2->next;
    int numTen = 10;
    while (p1 || p2){
      if(p1){
        n1 = p1->val*numTen+n1;
        p1 = p1->next;
      }
      if(p2){
        n2 = p2->val*numTen+n2;
        cout << n2 << endl;
        p2 = p2->next;
      }
      numTen *= 10;
    }
    int sum = n1+n2;
    ListNode* res = new ListNode(sum % 10);
    sum = sum / 10;

    ListNode* temp = res;
    while (sum > 0){
      temp->next = new ListNode(sum % 10);
      temp = temp->next;
      sum = sum / 10;
    }
    return res;
  }
};

// Second approach, this one is better because it solves the overflow problem
class Solution {
public:
  ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode* dummy = new ListNode(0);
    ListNode* p = dummy;
    int carry = 0;
    while(l1 || l2 || carry){
      if(l1){
        carry += l1->val;
        l1 = l1->next;
      }
      if(l2){
        carry += l2->val;
        l2 = l2->next;
      }
      p->next = new ListNode(carry%10);
      carry = carry/10;
      p = p->next;
    }
    return dummy->next;
  }
};

```
