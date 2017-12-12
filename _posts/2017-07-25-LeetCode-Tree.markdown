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

>2017-10-24

#### 94. Binary Tree Inorder Traversal
>Given a binary tree, return the inorder traversal of its nodes' values.  
For example:  
Given binary tree [1,null,2,3],  
   1  
    \  
     2  
    /  
   3  
return [1,3,2].  
Note: Recursive solution is trivial, could you do it iteratively?

`Thought Process:`
For this question the recursive solution is trivial, which is recursively traverse to the very left leaf, push the node value to the result vector, then push the mid node value, then recurse on the right edge.

The iterative solution is less trivial. We need to create a stack to keep track of the nodes traversed. We still traverse to the very left leaf, main while push node we have traversed to the stack. Once we reach very left of the tree, push the value to the result vector then get the node we pushed at the top of the stack which is the parent of the node that we just recored in the result vector. Now do the same process to the right node. Until our current node is NULL and there is no more node left in the stack (stack is empty), we know we have finished traverse the tree.

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
  vector<int> inorderTraversal(TreeNode* root) {
      vector<int> res;
      if (root == NULL) return res;
      iterative(root, res);
      return res;
  }

  // Recursive solution
  void recursive(TreeNode* tree, vector<int> &res) {
      if(tree->left == NULL && tree->right == NULL){
          res.push_back(tree->val);
          return ;
      }
      if (tree->left != NULL){
          recursive(tree->left, res);
      }
      res.push_back(tree->val);
      if (tree->right != NULL){
          recursive(tree->right, res);
      }
  }

  // Iterative solution
  void iterative(TreeNode* tree, vector<int> &res) {
    stack<TreeNode*> stack;
    TreeNode* cur = tree;
    while(cur || !stack.empty()){
      if (!cur){
        cur = stack.top();
        stack.pop();
        res.push_back(cur->val);
        cur = cur->right;
      }
      else {
        stack.push(cur);
        cur = cur->left;
      }
    }
  }
};
```

>2017-10-25

#### 230 Kth Smallest Element in a BST
>Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.
Note:   
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.  
Follow up:  
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

`Thought Process:`
Since we are dealing with a binary search tree, to find the Kth smallest element, we need to traverse to the very left leaf of the tree and back track to find the element. So it will be easier to do it iteratively compare recursive since we push node to stack so we can back track them.

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
  int kthSmallest(TreeNode* root, int k) {
    stack<TreeNode*> stack;
    TreeNode* curr = root;
    int res = curr->val;

    while (k > 0){
      if (curr == NULL) {
        curr = stack.top();
        stack.pop();
        res = curr->val;
        k--;
        curr = curr->right;
      }
      else {
        stack.push(curr);
        curr = curr->left;
      }
    }
    return res;
  }
};
```

>2017-10-26

#### 102. Binary Tree Level Order Traversal
>Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).  
For example:  
Given binary tree [3,9,20,null,null,15,7],  
    3  
   / \  
  9  20  
    /  \  
   15   7  
return its level order traversal as:  
[  
  [3],  
  [9,20],  
  [15,7]  
]  

`Thought Process:`
For this question, it's obvious to use iterative solution instead of recursive solution, because we need hold
information during our traversal. We need to record the node at the same level and do it to all levels in
the tree. The idea is to use a "queue" to store the nodes in the same level, and pop them once when push new nodes
from the next level.

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
 using namespace std;

 class Solution {
 public:
     vector<vector<int>> levelOrder(TreeNode* root) {
         vector<vector<int>> result;
         if (root == NULL) {
           return result;
         }

         queue<TreeNode* > Q;
         Q.push(root);

         while(!Q.empty()) {
           vector<int> level;
           int qsize = Q.size();

           for (int i = 0; i < qsize; i++){
             TreeNode* head = Q.front();
             level.push_back(head->val);
             Q.pop();
             if (head->left != NULL) {
               Q.push(head->left);
             }
             if (head->right != NULL) {
               Q.push(head->right);
             }
           }
           result.push_back(level);
         }
         return result;
     }
 };
```

>2017-10-27

#### 116. Populating Next Right Pointers in Each Node
>Given a binary tree  
    struct TreeLinkNode {  
      TreeLinkNode*left;  
      TreeLinkNode*right;  
      TreeLinkNode*next;
    }  
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.  
Initially, all next pointers are set to NULL.  
Note:  
You may only use constant extra space.  
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).  
For example,  
Given the following perfect binary tree,  
         1  
       /  \  
      2    3  
     / \  / \  
    4  5  6  7  
After calling your function, the tree should look like:  
         1 -> NULL  
       /  \  
      2 -> 3 -> NULL  
     / \  / \  
    4->5->6->7 -> NULL  

`Thought Process:`
The idea is to use three pointers, one to track the head of each level, one pointer traverse the level of the head, third pointer use the second pointer to do connecting operations for the next level.

```cpp
/**
 * Definition for binary tree with next pointer.  
 * struct TreeLinkNode {  
 *  int val;  
 *  TreeLinkNode *left, *right, *next;  
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}  
 * };  
 */
 class Solution {
 public:
     void connect(TreeLinkNode *root) {
         if (!root) return ;
         TreeLinkNode* head = root;
         while (head){
             TreeLinkNode* cur = head->left;
             if (cur == NULL) break;
             cur->next = head->right;
             cur = head->right;
             TreeLinkNode* row = head->next;
             while (row) {
                 cur->next = row->left;
                 row->left->next = row->right;
                 cur = row->right;
                 row = row->next;
             }
             head = head->left;
         }
     }
 };
```

>2017-11-12

#### 285. Inorder Successor in BST
>Given a binary search tree and a node in it, find the in-order successor of that node in the BST.  
Note: If the given node has no in-order successor in the tree, return null.

`Thought Process:`
The goal here is the find the node that is on the right of the p node, so all we need to do is remember the nood traverse left,
and we can use the BST property to traverse left if p->val is less than the current node, traverse right vise versa.

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
  TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
    if (root == NULL || p == NULL) {
      return NULL;
    }
    TreeNode* successor = NULL;
    while (root){
      if (p->val < root->val){
        successor = root;
        root = root->left;
      }
      else {
        root = root->right;
      }
    }
    return successor;
  }
}
```

>2017-11-14

#### 103. Binary Tree Zigzag Level Order Traversal
```
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its zigzag level order traversal as:
[
  [3],
  [20,9],
  [15,7]
]
```

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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (root == NULL) return vector<vector<int>>();
        vector<vector<int>> ans;
        ans.push_back(vector<int>(1, root -> val));
        // 记录上一层的结点情况
        vector<TreeNode*> lastlevel;
        lastlevel.push_back(root);
        // 一个基本的按层遍历方法
        int depth = 0;
        while (lastlevel.size() > 0) {
            vector<TreeNode*> newlevel;
            for (int j = 0; j < lastlevel.size(); j ++) {
                if (lastlevel[j] -> left != NULL) newlevel.push_back(lastlevel[j] -> left);
                if (lastlevel[j] -> right != NULL) newlevel.push_back(lastlevel[j] -> right);
            }
            lastlevel = newlevel;
            // 根据层数确定zigzag的方向，并构造该层的答案
            vector<int> intlevel;
            depth ++;
            if (depth % 2 == 0) {
                for (int j = 0; j < newlevel.size(); j ++) {
                    intlevel.push_back(newlevel[j] -> val);
                }
            }
            else {
                for (int j = newlevel.size() - 1; j >= 0; j--) {
                    intlevel.push_back(newlevel[j] -> val);
                }
            }
            // 加入答案
            if (intlevel.size() > 0) {
                ans.push_back(intlevel);
            }
        }
        return ans;
    }
};
```

#### 236. Lowest Common Ancestor of a Binary Tree
```
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```
`Thought Process:`
Recursive solution is trivial to implement for this question. From any node, we recursively iterate left and right tree, there are three cases
1. p and q is in different side of the tree, then node is the LCA
2. p and q both in the left tree (right tree returns NULL when searching for p and q), then whatever is returned from the iteration on left Tree
3. p and q both in the right tree (left tree returns NULL when searching for p and q), then whatever is returned from the iteration on right Tree

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL){
            return NULL;
        }
        if (root == p || root == q){
            return root;
        }

        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        if (left != NULL && right != NULL){
            return root;
        }
        else {
            return left != NULL ? left : right;
        }
    }
};
```
