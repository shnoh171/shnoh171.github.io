---
layout: post
title: Thread-Safe Linked List
categories:
  - Concurrency
---
The easiest way to make thread-safe linked list is to have a one single mutex to lock each operations (insert, delete and search). However, we cannot benefit from multithread programming since only one opearation can be runned at once. Alternatively, we can introduce more fine-grained locking: to have one mutex for each node in the linked list, and locks the mutex when the operation have to access its owner node. Basic class definition is as follows.

```c++
struct Node {
    int value;
    struct Node* next;
    mutex mtx_node;
    Node(int val): value(val), next(NULL) {}
};

class LinkedList {
    Node* head;
    mutex mtx_list;
public:
    LinkedList(): head(NULL) {}
    void insert_head(int value);
    void insert_n(int value, int n);
    void remove_n(int n);
    void print();
};
```

At this moment, let's assume that we only have to support `insert_head()` and `insert_n()` (forget about the deletion).
