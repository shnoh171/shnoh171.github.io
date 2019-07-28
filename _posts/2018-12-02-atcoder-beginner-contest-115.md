---
layout: post
title: AtCoder Beginner Contest 114
categories:
  - Problem Solving
---

Rating: **0** &rarr; **46**

I solved two out of the four problems. Almost solved the third problem too, could not finish it.

### A. 753

Problem: <https://abc114.contest.atcoder.jp/tasks/abc114_a>

My code:

```c++
#include <iostream>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int x;
	cin >> x;

	if (x == 7 || x == 5 || x == 3) cout << "YES\n";
	else cout << "NO\n";

	return 0;
}
```

The easiest problem I've ever seen in the competitive programming. I will skip the description.

### B. 754

Problem: <https://abc114.contest.atcoder.jp/tasks/abc114_b>

My code:

```c++
#include <iostream>
#include <string>
using namespace std;

const int max_val = 1000;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int diff_min = max_val;

	string s;
	cin >> s;

	int digits[s.size()];
	for (int i = 0; i < s.size(); ++i)
		digits[i] = s[i] - '0';

	for (int i = 0; i < s.size()-2; ++i) {
		int number = digits[i] * 100 + digits[i+1] * 10 + digits[i+2];
		diff_min = min(diff_min, abs(number-753));
	}

	cout << diff_min << "\n";

	return 0;
}
```

Simple linear search problem.

In the code above, it is not necessary to insert the characters of string `s` into the integer array `digits`. The linear search can be done directly in `s`. This will result more efficient and shorter code.
