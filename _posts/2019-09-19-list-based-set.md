---
layout: post
title: Thread-safe List-based Set
categories:
  - Concurrency
---
Implementing a find-grained locking mechanism is a bit hard in data structures such as linked list and tree. The main reason is dynamic allocation of nodes. We can utilize a locking technique called hand-over-hand locking. The main idea is to lock two mutexes simultaneously.

```c++
#include <iostream>
#include <thread>
#include <mutex>

#include <cstdlib>
#include <unistd.h>

using namespace std;

struct Node {
    int val;
    struct Node* next;
    mutex mtx_n;
    Node(int _val): val(_val), next(NULL) {}
};

class ListBasedSet {
    Node* head;
    mutex mtx_l;
public:
    ListBasedSet(): head(NULL) {}
    bool insert(int num);
    bool remove(int num);
    void print();
};

bool ListBasedSet::insert(int num) {
    mtx_l.lock();

    if (head == NULL) {
        head = new Node(num);

        mtx_l.unlock();

        return true;
    }

    (head->mtx_n).lock();

    if (num < head->val) {
        Node* temp = head;
        head = new Node(num);
        head->next = temp;

        mtx_l.unlock();
        (temp->mtx_n).unlock();

        return true;
    } else if (num == head->val) {

        mtx_l.unlock();
        (head->mtx_n).unlock();

        return false;
    }

    Node* prev = head;
    Node* curr = head->next;

    mtx_l.unlock();
    if (curr != NULL)
        (curr->mtx_n).lock();


    while (curr != NULL) {
        if (num < curr->val) {
            prev->next = new Node(num);
            prev->next->next = curr;

            (prev->mtx_n).unlock();
            (curr->mtx_n).unlock();

            return true;
        } else if (num == curr->val) {
            (prev->mtx_n).unlock();
            (curr->mtx_n).unlock();

            return false;
        }

        Node* temp = prev;

        prev = curr;
        curr = curr->next;

        (temp->mtx_n).unlock();
        if (curr != NULL)
            (curr->mtx_n).lock();
    }

    prev->next = new Node(num);

    (prev->mtx_n).unlock();
    if (curr != NULL)
        (curr->mtx_n).unlock();

    return true;
}

bool ListBasedSet::remove(int num) {
    mtx_l.lock();

    if (head == NULL) {
        mtx_l.unlock();

        return false;
    }

    (head->mtx_n).lock();

    if (num == head->val) {
        Node* temp = head;
        head = head->next;

        mtx_l.unlock();
        (temp->mtx_n).unlock();

        delete(temp);
        return true;
    }


    Node* prev = head;
    Node* curr = head->next;

    mtx_l.unlock();
    if (curr != NULL)
        (curr->mtx_n).lock();

    while (curr != NULL) {
        if (num == curr->val) {
            prev->next = curr->next;

            (prev->mtx_n).unlock();
            (curr->mtx_n).unlock();

            delete(curr);
            return true;
        }

        Node* temp = prev;

        prev = curr;
        curr = curr->next;

        (temp->mtx_n).unlock();
        if (curr != NULL)
            (curr->mtx_n).lock();
    }

    (prev->mtx_n).unlock();
    if (curr != NULL)
        (curr->mtx_n).unlock();

    return false;
}

void ListBasedSet::print() {
    Node* curr = head;
    while (curr != NULL) {
        cout << curr->val << " ";
        curr = curr->next;
    }
    cout << endl;
}
```
Testing functions:
```c++
ListBasedSet set;

void insertFunction() {
    for (int i = 0; i < 1000; ++i) {
        set.insert(i % 200);
    }
}

void deleteFunction() {
    for (int i = 0; i < 1000; ++i) {
        set.remove(i % 100);
    }
}

int main() {
    thread insertThread1(insertFunction);
    thread insertThread2(insertFunction);

    thread deleteThread1(deleteFunction);
    thread deleteThread2(deleteFunction);

    insertThread1.join();
    insertThread2.join();
    deleteThread1.join();
    deleteThread2.join();

    set.print();
}
```
