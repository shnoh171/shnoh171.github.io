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

**Can we do better? Of course!**

Let us assume that we are building BST by iteratively visiting the input sequence \\(S=(s_1, s_2, ..., s_n)\\). After finishing \\(i\\)th iteration, a set of visited nodes \\(\\{s_1, s_2, ..., s_i\\}\\) can be classified into three groups: (1) current node \\(\\{s_i\\}\\), (2) a set of nodes in \\(\\{s_1, s_2, ..., s_{i-1}\\}\\) that will not have \\(\\{s_{i+1}, s_{i+2}, ..., s_n\\}\\) as their child, and (3) a set of nodes in \\(\\{s_1, s_2, ..., s_{i-1}\\}\\) that can have \\(\\{s_{i+1}, s_{i+2}, ..., s_n\\}\\) as their child.

Note that the next node \\(s_{i+1}\\) should always be larger than (2) since this is BST.

We now iteratively visit input sequence and update (1), (2) and (3). While doing so, we check whether \\(s_{i+1}\\) is larger than (2). I will use a single integer variable to get the maximum value in (2), and a single stack to maintain (3).

The time complexity is \\(O(n)\\).

```c++
bool verifyPreorder(vector<int>& preorder) {
    stack<int> s;
    int bound = INT_MIN;

    for (auto node : preorder) {
        if (node < bound) return false;

        if (s.empty() || node < s.top()) {
            s.push(node);                
        } else {
            while (!s.empty() && node > s.top()) {
                bound = max(bound, s.top());
                s.pop();
            }
            s.push(node);
        }
    }

    return true;
}
```
