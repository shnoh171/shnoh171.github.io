---
layout: post
title: Largest Rectangle in a Histogram (University of Ulm Local Contest 2003)
categories:
  - Problem Solving
---
OJ: <https://www.spoj.com/BYUP4/problems/HISTOGRA/>

Let us denote the width of histogram as \\(n\\), and the height of the \\(i\\)th rectangle as \\(h_i\\).

For each \\(i\\)th rectangle, the area \\(a_i\\) of the largest rectangle that includes the \\(i\\)th rectangle is computed by \\(h_i \times (r_i - l_i + 1) \\). The \\(l_i\\) is the smallest index where \\(l_i\\)th rectangle through the \\(i\\)th rectangle have height greater than or equal to \\(h_i\\). The \\(r_i\\) is the largest index where \\(i\\)th rectangle through the \\(r_i\\)th rectangle have height greater than or equal to \\(h_i\\).

The final answer is the maximum value of \\(a_i\\) where \\(1 \le i \le n\\).

Now all I have to do is to compute each \\(a_i\\) in \\(O(1)\\). It can be done by using a stack. For each \\(i\\), I push the attributes of \\(i\\)th rectangle into the stack and compute \\(l_i\\). These attributes of the \\(i\\)th rectangle pop out from the stack when I can compute \\(r_i\\) in a constant time.

The detailed code is as follows.

```c++
#include <iostream>
#include <stack>
#include <vector>
#include <algorithm>
using namespace std;

struct element {
	int idx;
	int lidx;
	int height;

	element(int i, int l, int h) { idx = i; lidx = l; height = h; };
};

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	while (true) {
		int n;
		cin >> n;

		if (n == 0) break;

		vector<int> h(n);
		vector<int> left(n), right(n);
		for (int i = 0; i < n; ++i) cin >> h[i];

		stack<element> s;
		for (int i = 0; i < n; ++i) {
			int curr_height = h[i];

			while (!s.empty() && s.top().height > curr_height) {
				right[s.top().idx] = i - 1;
				s.pop();				
			}

			if (s.empty())
				left[i] = 0;
			else if (s.top().height == curr_height)
				left[i] = s.top().lidx;
			else
				left[i] = s.top().idx + 1;

			if (!s.empty() && s.top().height == curr_height)
				s.push(element(i, s.top().lidx, curr_height));
			else
				s.push(element(i, i, curr_height));
		}

		while (!s.empty()) {
			right[s.top().idx] = n - 1;
			s.pop();
		}

		long long res = 0;
		for (int i = 0; i < n; ++i)
			res = max(res, (long long) h[i] * (right[i] - left[i] + 1));
		cout << res << "\n";
	}


	return 0;
}
```
