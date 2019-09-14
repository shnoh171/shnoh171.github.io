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
    Node(int _value): value(_value), next(NULL) {}
};

class LinkedList {
    Node* head;
    mutex mtx_list;
public:
    LinkedList(): head(NULL) {}
    void insert_n(int value, int n);
    void remove_n(int n);
};
```

At this moment, let's assume that we only have to support `insert_n()` (forget about the deletion). `insert_n()` inserts a node with `value` after the `n`th index. To make things simple, I assume that `n` is always smaller than equal to the size of the linked list.

```c++
void LinkedList::insert_n(int value, int n) {
    mtx_list.lock();
    if (head == NULL) {
        head = new Node(value);
        mtx_list.unlock();
        return;
    }
    if (n == 0) {
        Node* temp = head;
        head = new Node(value);
        head->next = temp;
        mtx_list.unlock();
        return;
    }

    Node *curr = head;
    mtx_list.unlock();

    for (int i = 0; i < n - 1; ++i) {
        (curr->mtx_node).lock();
        Node *temp = curr;
        curr = curr->next;
        (temp->mtx_node).unlock();
    }

    (curr->mtx_node).lock();
    Node *temp = curr->next;
    curr->next = new Node(value);
    curr->next->next = temp;
    (curr->mtx_node).unlock();
}
```
There are two internal operations in `insert_n()` that should be atomic. The first operation is traverse from one node to another. While doing so, `insert_n()` (1) locks the current node's mutex, (2) move to the next node, and (3) unlocks the mutex. The second operation is inserting a node after the current node. The steps of the operations are (1) locks the current node's mutex, (2) adds a node after the current node, and (3) unlocks the mutex.

The first operation is basically read operation, while the second operation is write operation. We can validate the correctness of the algorithm by checking two cases: (1) when a read opeartion and a write operation run together, and (2) when two write operations run together. It is trivial that we do not have to care about the case when two read oeprations are runs together.
