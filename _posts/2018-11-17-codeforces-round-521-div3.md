---
layout: post
title: Codeforces Round 521 (Div.3)
categories:
  - Competitive Programming
---

Rating: 1517 &rarr; 1451

I solved three of the seven questions, and one of them was hacked. So my final standing is 2/7.

This is a very disappointing result for me. I think I could solve up to four problems. My mistakes were:

- I did not read the problem carefully and dive to the problem. I wasted much time due to misunderstanding the question.
- After solving the problem, I did not verify that the code was correct.

My thoughts to the problems up to E are as follows.

### A. Frog Jumping

Problem: <http://codeforces.com/contest/1077/problem/A>

My code:

```c++
#include <iostream>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);

	int t;
	cin >> t;

	for (int i = 0; i < t; ++i) {
		long long a, b, k;
		cin >> a >> b >> k;
		cout << ((k+1)/2)*a - (k/2)*b << "\n";
	}

	return 0;
}
```

Simple addition problem. The only thing to be careful about is that using int type variable for \\(x\\) can cause overflow. Note that the result can be up to about \\(\pm 5\times10^{17}\\) in the worst case.

### B. Disturbed People

Problem: <http://codeforces.com/contest/1077/problem/B>

My code:

```c++
#include <iostream>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);

	int n;
	cin >> n;
	int a[n];
	for (int i = 0; i < n; ++i) cin >> a[i];

	int cnt = 0;
	for (int i = 2; i < n; ++i) {
		if (a[i-2] == 1 && a[i-1] == 0 && a[i] == 1) {
			a[i] = 0;
			++cnt;
		}
	}

	cout << cnt << "\n";

	return 0;
}
```

Let us denote a set of flats that turn off the light as \\(\{x_1, x_2, ..., x_k\}\\). Assume without loss of generality that \\(x_1 \le x_2 \le ... \le x_k\\)


### C. Good Array

### D. Cutting Out

### E. Thematic Contest
