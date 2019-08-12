---
layout: post
title: Producer-Consumer Problems in Python
categories:
  - Problem Solving
---
### Using Semaphore
When the buffer size is one, two semaphores will solve the problem.
```python
import threading
import random
import time

queue = []
queueIsAvailable = threading.Semaphore(1)
dataIsAvailable = threading.Semaphore(0)

def producer():
    nums = range(5)
    global queue
    while True:
        num = random.choice(nums)

        queueIsAvailable.acquire()

        queue.append(num)
        print("Produced", num, queue)

        dataIsAvailable.release()

        time.sleep(random.randrange(0, 3))

def consumer():
    global queue
    while True:
        dataIsAvailable.acquire()

        num = queue.pop(0)
        print("Consumed", num, queue)

        queueIsAvailable.release()

        time.sleep(random.randrange(0, 3))

producerThread = threading.Thread(target=producer)
consumerThread = threading.Thread(target=consumer)

producerThread.start()
consumerThread.start()
```
When the buffer size is greater than one, this solution will not work since there is a race condition (Multiple processes accessing the buffer (queue) at the same time).

Adding a mutex (I used Python's Lock primitive) will solve the problem.
```python
import threading
import random
import time

queue = []
queueIsAvailable = threading.Semaphore(5)
dataIsAvailable = threading.Semaphore(0)
mutex = threading.Lock()

def producer():
    nums = range(5)
    global queue
    while True:
        num = random.choice(nums)

        queueIsAvailable.acquire()

        mutex.acquire()         # added

        queue.append(num)
        print("Produced", num, queue)

        mutex.release()         # added

        dataIsAvailable.release()

        time.sleep(random.randrange(0, 3))

def consumer():
    global queue
    while True:
        dataIsAvailable.acquire()

        mutex.acquire()         # added

        num = queue.pop(0)
        print("Consumed", num, queue)

        mutex.release()         # added

        queueIsAvailable.release()

        time.sleep(random.randrange(0, 3))

producerThread = threading.Thread(target=producer)
consumerThread = threading.Thread(target=consumer)

producerThread.start()
consumerThread.start()
```
### Using Condition
We can use a condtion variable supported by Python to solve the problem. Note that we used a condition variable to suspend both producer thread and consumer thread.
```python
import threading
import random
import time

queue = []
MAX_SIZE = 5
cv = threading.Condition()

def producer():
    nums = range(5)
    global queue
    while True:
        num = random.choice(nums)

        cv.acquire()

        while len(queue) >= MAX_SIZE:
                cv.wait()

        queue.append(num)
        print("Produced", num, queue)

        cv.notify()

        cv.release()

        time.sleep(random.randrange(0, 3))

def consumer():
    global queue
    while True:

        cv.acquire()

        while len(queue) < 1:
                cv.wait()

        num = queue.pop(0)
        print("Consumed", num, queue)

        cv.notify()

        cv.release()

        time.sleep(random.randrange(0, 3))

producerThread = threading.Thread(target=producer)
consumerThread = threading.Thread(target=consumer)

producerThread.start()
consumerThread.start()
```
