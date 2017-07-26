---
layout:     post
title:      "LeetCode: Top Interview Questions - Tree"
subtitle:   "这里是Tree类型题的锦集"
date:       2017-07-25 12:00:00
author:     "YuXiao"
header-img: "img/post-bg-miui6.jpg"
header-mask: 0.3
catalog:    true
tags:
    - LeetCode
    - Top Interview Questions
    - LeetCode-Tree
---

>2017-07-24

#### 101. Symmetric Tree
>Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).  
For example, this binary tree [1,2,2,3,4,4,3] is symmetric:  
    1  
   / \  
  2   2  
 / \ / \  
3  4 4  3  
But the following [1,2,2,null,3,null,3] is not:  
    1  
   / \  
  2   2  
   \   \  
   3    3  
Note:  
Bonus points if you could solve it both recursively and iteratively.  

`Thought Process:`  
A simple solution will be use two array to traverse the left and right tree, and check if the arrays are anagrams. But that would take a lot of extra spaces. The better algorithm would be do dfs on the left and right tree using opposite traverse directions, and check if the value is the same and recursively check the left and right node.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        return dfs(root, root);
    }

    bool dfs(TreeNode* root1, TreeNode* root2){
        if (root1->val != root2->val) return false;
        if (root1->left != NULL){
            if (root2->right == NULL) return false;
            if (!dfs(root1->left, root2->right)) return false;
        }
        if (root1->right != NULL){
            if (root2->left == NULL) return false;
            if (!dfs(root1->right, root2->left)) return false;
        }
        return true;
    }
};
```

#### 104. Maximum Depth of Binary Tree
>Given a binary tree, find its maximum depth.  
The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

`Thought Process:`  
Typical recursion question. Count maximum nodes number traversed.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL){
            return 0;
        }
        else {
            return max(maxDepth(root->left)+1, maxDepth(root->right)+1);
        }
    }

};
```

#### 108. Convert Sorted Array to Binary Search Tree
>Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

`Thought Process:`  
We know the mid in the array will be the root of the tree, so just take mid as the root and recurse the same operation on the left side of mid and right side of mid in the array.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums, 0, nums.size()-1);
    }

    TreeNode* helper(vector<int>& nums, int left, int right){
        if (left > right){
            return NULL;
        }
        int mid = (left + right)/2;
        TreeNode* node = new TreeNode(nums[mid]);
        node->left = helper(nums, left, mid-1);
        node->right = helper(nums, mid+1, right);
        return node;
    }
};
```
