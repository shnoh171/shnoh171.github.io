---
layout: post
title: Implementing Synchronization Primitives
categories:
  - Concurrency
---
In this post, we will conceptually implement four basic synchronization primitives in C: (1) spinlock, (2) mutex, (3) semaphore and (4) condition variables. The codes are somewhat like pseudo code, and I will not actually run it.

### Spinlock

Spinlock can be implemented with hardware-assisted atomic operations such as `test_and_set()` and `compare_and_swap()`. The codes are as follow.

```c++
bool test_and_set(bool* target) {
    // executed atomically
    bool ret = *target;
    *target = true;
    return ret;
}

struct spinlock {
    bool flag = false;
};

void spin_lock(struct spinlock* lock) {
    while (test_and_set(&(lock->flag))) {}
}

void spin_unlock(struct spinlock* lock) {
    flag = false;
}
```

```c++
bool compare_and_swap(int* value, int expected, int new_value) {
    // executed atomically
    int temp = *value;
    if (*value == expected)
        *value = new_value;
    return temp;
}

struct spinlock {
    int flag = 0;
};

void spin_lock(struct spinlock* lock) {
    while (compare_and_swap(&(lock->flag), 0, 1)) {}
}

void spin_unlock(struct spinlock* lock) {
    lock->flag = 0;
}
```

// TODO: Ticket Spinlock

// TODO: Mutex

### Semaphore

```c++
struct semaphore {
    struct spinlock lock;
    int count;
    struct queue wait_queue;
};

void wait(struct semaphore* sem) {
    spin_lock(&(sem->lock));
    if (sem->count > 0) {
        --(sem->count);
        spin_unlock(&(sem->lock));
    } else {
        --(sem->count);
        enqueue_current_thread(&(sem->wait_queue));
        spin_unlock(&(sem->lock));
        sleep_current_thread();
    }
}

void signal(struct semaphore* sem) {
    spin_lock(&(sem->lock));
    if (sem->count >= 0) {
        ++(sem->count);
    } else {
        ++(sem->count);
        dequeue_and_wake_up(&(sem->wait_queue));
    }
    spin_unlock(&(sem->lock));
}
```
In order for this functions to work correctly, there is an implementation issues in `sleep_current_thread()` and `dequeue_and_wake_up()` functions. Basically I tried to make all the codes of `wait()` and `signal()` to be critical sections, but there is one exception: `sleep_current_thread()`. I have to release the `spinlock` before blocking the current thread! As consequence, there might be a condition where a thread calls `wait()` is about to call `sleep_current_thread()`, and another thread calls `signal()` and `dequeue_and_wake_up()`. If the `wait_queue` has only one thread, it has to defer `dequeue_and_wake_up()` until `sleep_current_thread()` is called.
