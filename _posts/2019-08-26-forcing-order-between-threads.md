---
layout: post
title: Forcing Order between Threads
categories:
  - Concurrency
---
The `condition_variable` class is a synchronization primitive that can be used to block a thread, or multiple threads at the same time, until another thread both modifies a shared variable (the condition), and notifies the `condition_variable` [^CV]. `atomic` template defines an atomic type. If one thread writes to an atomic object while another thread reads from it, the behavior is well-defined [^A] (see memory model [^MM] for details on data races).

I will use these two language constructs to force order between threads while accessing a shared data structure. The codes are based on the book named Concurrency with Modern C++ [^Rainer17].

```c++
#include <iostream>
#include <vector>
#include <algorithm>

#include <thread>
#include <mutex>
#include <condition_variable>

using namespace std;

vector<int> shared_work;
mutex mu;
condition_variable cv;

bool data_ready = false;

void sortData() {
    cout << "Waiting for Data" << endl;
    unique_lock<mutex> lck(mu);
    cv.wait(lck, [] { return data_ready; });
    sort(shared_work.begin(), shared_work.end());
    cout << "Sort Complete" << endl;
}

void setData() {
    shared_work = {2, 1, 5, 3, 4};
    {
        lock_guard<mutex> lck(mu);
        data_ready = true;
    }
    cout << "Data Prepared" << endl;
    cv.notify_one();
}

int main() {
    thread t1(sortData);
    thread t2(setData);

    t1.join();
    t2.join();

    for (auto v: shared_work) {
        cout << v << " ";
    }
    cout << endl;
}
```
This is a simple multi-threaded program where a thread `t1` sets a vector, and a thread `t2` sorts the vector. A condition variable `cv` is used to force `t2` to wait until `t1` finishes its work (push).

```c++
#include <iostream>
#include <vector>
#include <algorithm>

#include <thread>
#include <atomic>
#include <chrono>

using namespace std;

vector<int> shared_work;
atomic<bool> data_ready(false);

void sortData() {
    cout << "Waiting for Data" << endl;
    while (!data_ready.load()) {
        this_thread::sleep_for(chrono::milliseconds(5));
    }
    sort(shared_work.begin(), shared_work.end());
    cout << "Sort Complete" << endl;
}

void setData() {
    shared_work = {2, 1, 5, 3, 4};
    data_ready = true;
    cout << "Data Prepared" << endl;
}

int main() {
    thread t1(sortData);
    thread t2(setData);

    t1.join();
    t2.join();

    for (auto v: shared_work) {
        cout << v << " ";
    }
    cout << endl;
}
```
In this code, we used `atomic<bool>` variable for `t2` to busy-wait until `t1` finishes its job (pull).

[^CV]: <https://en.cppreference.com/w/cpp/thread/condition_variable>
[^A]: <https://en.cppreference.com/w/cpp/atomic/atomic>
[^MM]: <https://en.cppreference.com/w/cpp/atomic/memory_order>
[^Rainer17]: Rainer Grimm, Concurrency with Modern C++, 2017.
