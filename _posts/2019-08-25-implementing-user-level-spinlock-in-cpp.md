---
layout: post
title: Implementing User-Level Spinlock in C++
categories:
  - Concurrency
---
```c++
#include <iostream>
#include <atomic>
#include <thread>
using namespace std;

class Spinlock {
    atomic_flag flag;
public:
    Spinlock(): flag(ATOMIC_FLAG_INIT) {}
    void lock() { while (flag.test_and_set()); }
    void unlock() { flag.clear(); }
};

// Testing Spinlock

Spinlock l;
int count = 0;

void workOnResource() {
    for (int i = 0; i < 10; ++i) {
        l.lock();
        cout << ++count << endl;
        l.unlock();
    }
}

int main() {
    thread t1(workOnResource);
    thread t2(workOnResource);

    t1.join();
    t2.join();
}
```

### Reference
- Rainer Grimm, Concurrency with Modern C++, 2017.
- https://github.com/Jpub/RainerGrimm
