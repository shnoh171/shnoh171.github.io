---
layout: post
title: Quick Sort
categories:
  - Problem Solving
---
A basic quick sort algorithm implemented in C++.

```c++
void swap (int* a, int* b) {
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

void quicksort(int arr[], int left, int right) {
    int i, last;

    if (left >= right) return;

    // If there exist a pivot selection policy,
    // do it in this place.

    last = left;

    for (i = left + 1; i <= right; ++i) {
        if (arr[i] <= arr[left]) {
            swap(&arr[++last], &arr[i]);
        }
    }
    swap(&arr[left], &arr[last]);
    quicksort(arr, left, last - 1);
    quicksort(arr, last + 1, right);
}
```
