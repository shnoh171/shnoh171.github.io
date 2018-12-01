---
layout: post
title: Educational Codeforces Round 55 (Div.2)
categories:
  - Competitive Programming
---

Rating: **1451** &rarr; **1498**

I solved two out of the seven questions. It took about an hour to code first two problem. The key idea to solve the problem D was easy, but it almost took an hour to implement it. I ran out of time while debugging.

In order to achive better results, I must focus on reducing the implementation time.

After the contest, I solved the problems up to D. I post the results below.


###  A. Vasya and Book

Problem: <https://codeforces.com/contest/1082/problem/A>

My code:

```c++
#include <iostream>
#include <cstdlib>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int t;
	cin >> t;

	for (int i = 0; i < t; ++i) {
		int n, x, y, d;
		cin >> n >> x >> y >> d;

		if (abs(x-y) % d == 0) {
			cout << abs(x-y) / d << '\n';
		} else if (abs(y-1) % d == 0 && abs(n-y) % d != 0) {
			int move = abs(x-1) / d + 1 + abs(y-1) / d;
			cout << move << '\n';
		} else if (abs(y-1) % d != 0 && abs(n-y) % d == 0) {
			int move = abs(n-x) / d + 1 + abs(n-y) / d;
			cout << move << '\n';
		} else if (abs(y-1) % d == 0 && abs(n-y) % d == 0) {
			int move1 = abs(x-1) / d + 1 + abs(y-1) / d;
			int move2 = abs(n-x) / d + 1 + abs(n-y) / d;
			cout << min(move1, move2) << '\n';
		} else {
			cout << "-1\n";
		}
	}

	return 0;
}
```

If \\( \lvert y-x \rvert \\) is divided by \\(d\\), the optimal solution is to go directly from \\(x\\) to \\(y\\).

If not, I should find out whether \\( \lvert y-1 \rvert \\) or \\( \lvert \(n-1\)-y \rvert \\) is divided by \\(d\\). If this is the case, the solution is to go to the page \\(1\\) or \\(n-1\\), respectively, then go to \\(y\\). If both conditions hold, we choose shorter path.


