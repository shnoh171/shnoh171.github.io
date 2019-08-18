---
layout: post
title: Verify Preorder Sequence in BST
categories:
  - Problem Solving
---
**Problem: Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree. You may assume each number in the sequence is unique.**

<https://leetcode.com/problems/verify-preorder-sequence-in-binary-search-tree/>

First, we can think of simple \\(O(nd)\\) solution where \\(n\\) is the number of nodes and \\(d\\) is the depth of the BST. The algorithm recursively divides the given sequence into three parts: (1) root node, (2) nodes of the left subtree and (3) nodes of the right subtree. By definition of the preorder traversal and BST, (1) is the rightmost element in the sequence. Since (2) should be less than (1) and (3) should be larger than (1), we could easily determine (2) and (3) within \\(O(n)\\). If we cannot divide the given sequence into three parts, it means that the input is not the correct preorder traversal sequence of a BST, thus we return false.

```c++
bool verifyPreorder(vector<int>& preorder) {
    return _verifyPreorder(preorder, 0, preorder.size() - 1);
}

bool _verifyPreorder(vector<int>& preorder, int left, int right) {
    if (left >= right) return true;

    bool ret = true;
    int head = preorder[left];
    int pivot = right + 1;
    bool is_small = true;

    for (int i = left + 1; i <= right; ++i) {
        if (is_small) {
            if (preorder[i] > head) {
                pivot = i;
                is_small = false;
            }
        } else {
            if (preorder[i] < head) {
                return false;
            }
        }
    }

    ret = ret && _verifyPreorder(preorder, left + 1, pivot - 1);
    ret = ret && _verifyPreorder(preorder, pivot, right);

    return ret;
}
```
However, this algorithm's time complexity becomes \\(O(n^2)\\) when the BST is not balanced.

Can we do better? Yes!

We can divide operations on preorder traversal into two types: (1) visit the left child and (1) going up until the node has right child and visit it.
