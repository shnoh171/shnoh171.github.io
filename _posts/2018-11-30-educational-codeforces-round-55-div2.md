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

### A. Vasya and Book

Problem: <https://codeforces.com/contest/1082/problem/A>

If \\( \lvert y-x \rvert \\) is divided by \\(d\\), the optimal solution is to go directly from \\(x\\) to \\(y\\).

If not, I should find out whether \\( \lvert y-1 \rvert \\) or \\( \lvert \(n-1\)-y \rvert \\) is divided by \\(d\\). If so, the solution is to go to the page \\(1\\) or \\(n-1\\), respectively, then go to \\(y\\). If both conditions hold, we choose shorter path.

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

### B. Vova and Trophies

Problem: <https://codeforces.com/contest/1082/problem/B>

Let us divide cases based on the number \\(m\\) of golden trophies subsegment.

If \\(m=0\\), the answer is \\(0\\).

If \\(m=1\\), the answer is equal to the length of the unique golden trophies subsegment.

If \\(m=2\\), count the number of silver trophies between two golden trophies subsegments. If it is \\(1\\), the answer is sum of the lengths of two golden trophies subsegments. If it is larger than \\(1\\), the answer is the length of the larger golden trophies subsegment plus one.

If \\(m>2\\), the algorithm should go through all trophies, and find the longest possible answer. The logic of each check is similar to the \\(m=2\\) case.

```c++
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int n;
	string s;
	cin >> n >> s;

	int start_of_gold = 0;
	while (s[start_of_gold] == 'S') ++start_of_gold;

	if (start_of_gold >= n) {
		cout << "0\n";
		return 0;
	} else if (start_of_gold == n-1) {
		cout << "1\n";
		return 0;
	}

	vector<int> golds, silvers;
	bool gold_turn = true;
	int gold_count = 1, silver_count = 0;

	for (int i = start_of_gold + 1; i < n; ++i) {
		if (gold_turn) {
			if (s[i] == 'G') {
				++gold_count;
			} else {
				golds.push_back(gold_count);
				gold_count = 0;
				silver_count = 1;
				gold_turn = false;
			}
		} else {
			if (s[i] == 'S') {
				++silver_count;
			} else {
				silvers.push_back(silver_count);
				silver_count = 0;
				gold_count = 1;
				gold_turn = true;
			}
		}
	}

	if (gold_count != 0) golds.push_back(gold_count);
	if (silver_count != 0) silvers.push_back(silver_count);
	if (golds.size() != silvers.size()) silvers.push_back(0);

	if (golds.size() == 0) {
		cout << "0\n";
	} else if (golds.size() == 1) {
		cout << golds[0] << '\n';
	} else if (golds.size() == 2) {
		if (silvers[0] == 1)  {
			cout << golds[0] + golds[1] << '\n';
		} else {
			cout << max(golds[0], golds[1]) + 1 << '\n';
		}
	} else {
		int max_beauty = 0;

		for (int i = 0; i < golds.size() - 1; ++i) {
			if (silvers[i] == 1) {
				max_beauty = max(golds[i] + golds[i+1] + 1, max_beauty);
			} else {
				max_beauty = max(golds[i] + 1, max_beauty);
			}
		}

		max_beauty = max(golds[golds.size()-1] + 1, max_beauty);

		cout << max_beauty << '\n';
	}

	return 0;
}
```

However, I found a solution that is much easier to implement: <https://codeforces.com/blog/entry/63544>

### C.

Problem: <https://codeforces.com/contest/1082/problem/C>

### D. Maximum Diameter Graph

Problem: <https://codeforces.com/contest/1082/problem/D>


