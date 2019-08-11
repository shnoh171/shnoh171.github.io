---
layout: post
title: Producer-Consumer Problems in Python
categories:
  - Problem Solving
---
### Using Semaphore (Buffer Size is one)
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

        num = queue.pop()
        print("Consumed", num, queue)

        queueIsAvailable.release()

        time.sleep(random.randrange(0, 3))

producerThread = threading.Thread(target=producer)
consumerThread = threading.Thread(target=consumer)

producerThread.start()
consumerThread.start()
```
When buffer size is greater than one, this solution will not work since there is a race condition (Multiple processes accessing the buffer (queue) at the same time).
