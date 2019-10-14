---
layout: post
title: Minimizing Difference
categories:
  - Problem Solving
---

<https://codeforces.com/contest/1244/problem/E>

Fresh problem from the Codeforces contest held yesterday!

First of all, I will sort the sequence of integers.

Now I will introduce an iterative algorithm. For each iteration, the algorithm tries to reduce the difference between the maximum and minimum elements in the sequence. There are two possible operations.

1. Increase all minimum elements
2. Decrease all maximum elements

The algorithm checks the cost for each operation and chooses the cheaper one.

The codes are as follows.

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int n;
	long long k;
	cin >> n >> k;

	vector<int> a(n);
	for (int i = 0; i < n; ++i) {
		cin >> a[i];
	}
	sort(a.begin(), a.end());

	int left = 0;
	int right = n - 1;

	while (left < right) {
		if (a[left] == a[left+1]) {
			++left;
			continue;
		} else if (a[right] == a[right-1]) {
			--right;
			continue;
		}

		long long inc_cost = left + 1;
		long long dec_cost = n - right;

		if (inc_cost <= dec_cost) {
			long long inc_amount = (a[left+1] - a[left]) * inc_cost;
			if (inc_amount <= k) {
				k -= inc_amount;
				++left;
			} else {
				long long partial_inc = k / inc_cost;
				cout << a[right] - a[left] - partial_inc << "\n";
				return 0;
			}
		} else {
			long long dec_amount = (a[right] - a[right-1]) * dec_cost;
			if (dec_amount <= k) {
				k -= dec_amount;
				--right;
			} else {
				long long partial_dec = k / dec_cost;
				cout << a[right] - a[left] - partial_dec << "\n";
				return 0;
			}
		}
	}

	cout << "0\n";

	return 0;
}
```

The time complexity is \\(O(n log n)\\) (sorting).
