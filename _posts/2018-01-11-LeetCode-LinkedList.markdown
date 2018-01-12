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
