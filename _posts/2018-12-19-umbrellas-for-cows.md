---
layout: post
title: Umbrellas for Cows (USACO December 2012 Silver Div.)
categories:
  - Competitive Programming
---

Problem: <http://www.usaco.org/index.php?page=viewproblem2&cpid=99>

This is a dynamic programming problem. I define \\(d_{i}\\) as the minimum cost it takes to protect all cows numbered from \\(1\\) to \\(i\\). The proposed algorithm computes \\(d_i\\) from \\(i=1\\) to \\(n\\). The final solution is \\(d_n\\).

The most import observation to solve this problem is that the larger umbrella can be cheaper than the smaller umbrella. Using the cost \\(c_w\\) to buy an umbrella of width \\(w\\), I additionally define \\(c^\prime_w\\) that is the cost to buy the cheapest umbrella that can protects the area with width \\(w\\). It is computed using the formula below.

\\[c_w^{\prime} = \min_{w \le i \le m} (c_{w})\\]

where \\(m\\) is the length of the stall.

Now let us start computing \\(d_{i}\\) using \\(c^\prime_w\\). \\(d_1\\) is equal to \\(c^\prime_1\\) by definition. Remaining \\(d_{i}\\)s are calculated as follows.

\\[d_{i}= \min (d_{i-1}, c^\prime_w\\]

...




where 2 \le i \le m and C^'_i is the minimum value of C_k where 1 \le k \le m

wefwe


```c++
#include <iostream>
#include <algorithm>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int n, m;
	cin >> n >> m;

	int x[n], c[m+1];
	for (int i = 0; i < n; ++i) cin >> x[i];
	for (int i = 1; i < m+1; ++i) cin >> c[i];
	sort(x, x+n);

	int min_c[m+1];
	min_c[m] = c[m];
	for (int i = m-1; i >= 0; --i) {
		min_c[i] = min(min_c[i+1], c[i]);
	}

	int d[n];
	d[0] = min_c[1];
	for (int i = 1; i < n; ++i) {
		d[i] = min_c[x[i]-x[0]+ 1];
		for (int j = 0; j < i; ++j) {
			d[i] = min(d[i], d[j] + min_c[x[i]-x[j+1]+1]);
		}
	}

	cout << d[n-1] << "\n";

	return 0;
}
```
