---
layout: post
title: Codeforces Round 521 (Div.3)
categories:
  - Competitive Programming
---

Rating: **1517** &rarr; **1451**

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

Simple addition problem. The only thing to be careful about is that using int type variable for \\(x\\) can cause overflow. Note that the result can be up to about \\(\\pm 5 \times 10^{17}\\) in the worst case.

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

I made three assumptions to solve the problem.

1. Without loss of generality, I can turn off the light of flats from left to right. I denote a set of these flats by \\( X = \\{ x_1, x_2, ..., x_k \\} \\) where \\(x_1 \le x_2 \le ... \le x_k\\).
2. There exists an optimal solution \\(X\\) even if I never turn off the light until I find a flat \\(a_i\\) such that \\(a_{i-2} = 1\\), \\(a_{i-1} = 0\\), and \\(a_i = 1\\). I will turn off the light of one flat among these three flats.
3. While \\(a_{i-2} = 1, a_{i-1} = 0, a_i = 1\\), turning off the light of \\(a_i\\) always yields the optimal solution.

The first assumption is a common mathematical technique. The second assumption is also obvious since we do not have to turn off the light unless there is a person who is "disturbed and cannot sleep".

The third assumption might be tricky in the first place, but it is also obvious. There is a chance to make two people not "disturbed and cannot sleep" at the same time only when \\(a_i\\) is turned off. This case happens when \\(a_{i+1} = 0\\} and \\{a_{i+2} = 1\\}.

### C. Good Array

### D. Cutting Out

### E. Thematic Contest
