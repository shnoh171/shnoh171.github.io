---
layout: post
title: Nested List Weight Sum
categories:
  - Problem Solving
---

<https://leetcode.com/problems/nested-list-weight-sum/>

The nested class is defined as follows.

```c++
/**
 * // Check link for the detail
 * class NestedInteger {
 *   public:
 *     NestedInteger();
 *     NestedInteger(int value);
 *     bool isInteger() const;
 *     int getInteger() const;
 *     void setInteger(int value);
 *     void add(const NestedInteger &ni);
 *     const vector<NestedInteger> &getList() const;
 * };
 */
```

This is basic recursive function. I coded as follows.

```c++

int _depthSum(const NestedInteger& nestedInteger, int depth) {
  if (nestedInteger.isInteger()) {
    return depth * nestedInteger.getInteger();
  }

  int ret = 0;

  for (auto next: nestedInteger.getList())
    ret += _depthSum(next, depth+1);

  return ret;
}

int depthSum(vector<NestedInteger>& nestedList) {
  if (nestedList.empty()) return 0;
  int ret = 0;

  for (auto next: nestedList) {
    ret += _depthSum(next, 1);
  }

  return ret;
}
```

<https://leetcode.com/problems/nested-list-weight-sum-ii/>

Now the problem is slightly changed, and weight is increasing from root to leaf. One idea might be compute the depth of the tree first, than use that information to weight of each node.

```c++
int computeDepth(const NestedInteger& nestedInteger) {
  if (nestedInteger.isInteger()) return 1;

  int ret = 0;
  for (auto next: nestedInteger.getList()) {
    ret = max(ret, computeDepth(next) + 1);
  }

  return ret;
}

int _depthSumInverse(const NestedInteger& nestedInteger, int depth) {
  if (nestedInteger.isInteger()) {
    return depth * nestedInteger.getInteger();
  }

  int ret = 0;

  for (auto next: nestedInteger.getList())
    ret += _depthSumInverse(next, depth-1);

  return ret;
}

int depthSumInverse(vector<NestedInteger>& nestedList) {
  if (nestedList.empty()) return 0;
  int ret = 0;
  int max_depth = 0;

  for (auto next: nestedList) {
    max_depth = max(max_depth, computeDepth(next));
  }

  for (auto next: nestedList) {
    ret += _depthSumInverse(next, max_depth);
  }

  return ret;
}
```

Another approach would be compute the result with virtual depth which is started from -1 at the root, and decreases by one as it goes to the lower level. We keep track of summation of the list and maximum depth, and use the information to correct the result.

```c++

int _depthSumInverse(const NestedInteger& nestedInteger, int vdepth, int& sum, int& max_depth) {
  if (nestedInteger.isInteger()) {
    sum += nestedInteger.getInteger();
    max_depth = max(max_depth,-vdepth);
    return vdepth * nestedInteger.getInteger();
  }

  int ret = 0;

  for (auto next: nestedInteger.getList())
    ret += _depthSumInverse(next, vdepth-1, sum, max_depth);

  return ret;
}

int depthSumInverse(vector<NestedInteger>& nestedList) {
  if (nestedList.empty()) return 0;
  int ret = 0;
  int max_depth = 0;
  int sum = 0;

  for (auto next: nestedList) {
    ret += _depthSumInverse(next, -1, sum, max_depth);
  }

  ret += (max_depth + 1) * sum;

  return ret;
}
```
