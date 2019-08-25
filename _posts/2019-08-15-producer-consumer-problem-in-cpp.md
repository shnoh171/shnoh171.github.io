---
layout: post
title: Producer-Consumer Problem in C++
categories:
  - Concurrency 
---
Used `mutex` and `condition_variable`. While using them, I utilized `unique_lock` class that provides a flexible way to lock/unlock mutexes and control condition variables.
```c++
#include <iostream>
#include <queue>
#include <thread>
#include <mutex>
#include <condition_variable>

#include <cstdlib>
#include <unistd.h>

#define MAX_SIZE 4

using namespace std;

queue<int> q;
condition_variable cv;
mutex mu;

void producer(int n) {
    for (int i = 0; i < n; ++i) {
        unique_lock<mutex> lock(mu);

        cv.wait(lock, []{return q.size() < MAX_SIZE;});

        q.push(i);
        cout << "Produced: " << i;
        cout << " Queue Size: " << q.size() << endl;

        cv.notify_one();

        lock.unlock();

        sleep(rand() % 3);
    }
}

void consumer(int n) {
    for (int i = 0; i < n; ++i) {
        unique_lock<mutex> lock(mu);

        cv.wait(lock, []{return !q.empty();});

        int value = q.front();
        q.pop();
        cout << "Consumed: " << i;
        cout << " Queue Size: " << q.size() << endl;

        cv.notify_one();

        lock.unlock();

        sleep(rand() % 5);
    }
}

int main() {
    thread producer_thread(producer, 20);
    thread consumer_thread(consumer, 20);

    producer_thread.join();
    consumer_thread.join();
}
```
You might want to use `lock_guard` class that is less flexible but provides more structured and safer way to lock/unlock a mutex. It locks the mutex on construction and unlocks it on destruction.
