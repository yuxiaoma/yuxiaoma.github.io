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
