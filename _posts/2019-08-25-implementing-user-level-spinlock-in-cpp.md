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
When the `lock()` method is called by a thread, the thread keeps spinning until it acquires the spinlock (`flag.test_and_set()` returns true).

Note that there might be starvation if many threads are competing for a single `Spinlock`. If we implement ticket spinlock mechanism then we can solve the problem.

Things to remember when using `atomic_flag`:
- `atomic_flag` should always be initialize to false using `ATOMIC_FLAG_INIT`.
- `atomic_flag` provides two methods: `test_and_set()` and `clear()`.
- It also provides the assignment operator (=), but I did not used it in this example.

### Reference
- Rainer Grimm, Concurrency with Modern C++, 2017.
- <https://github.com/Jpub/RainerGrimm>
