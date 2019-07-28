---
layout: post
title: Ramp Number (ACM-ICPC Pacific NW Regional 2014 Div. 2)
categories:
  - Problem Solving
---

Problem: <http://acmicpc-pacnw.org/ProblemSet/2014/div2.pdf>

I define \\(d_{i, j}\\) as the number of ramp numbers that is smaller than the number consists of the most significant \\(i\\) digits of the input, and the \\(i\\)th digit is \\(j\\). The \\(d_{i, j}\\) is computed as below.

The initialization \\((i=1)\\):

- \\(d_{1, j} = 1\\) if \\(j\\) is smaller than the most significant digit of the input.
- \\(d_{1, j} = 0\\) otherwise.

For \\(i \ge 2 \\), if \\(l_{i-1} \le j \lt l_{i} \\) where \\(l_i\\) is \\(i\\)th significant digit of the input,

\\[d_{i, j} = \sum_{k=1}^{j} d_{i-1, k} + 1\\]

If not,

\\[d_{i, j} = \sum_{k=1}^{j} d_{i-1, k}\\]

After computing all \\(d_{i, j}\\), the answer is sum of \\(d_{i, j}\\) for \\(0 \le j \le 9\\) and \\(i=m\\) where \\(m\\) is the number of digits of the input.

The code is as follows.

```c++
#include <iostream>
#include <vector>
#include <string>
using namespace std;

bool IsRampNumber(string num);
long long GetNumOfLessRampNumber(string num);

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int n;
	cin >> n;

	for (int i = 0; i < n; ++i) {
		string num;
		cin >> num;
		if (!IsRampNumber(num)) {
			cout << "-1\n";
		}
		else {
			cout << GetNumOfLessRampNumber(num) << "\n";
		}
	}

	return 0;
}

bool IsRampNumber(string num)
{
	for (int i = 0; i < num.size() - 1; ++i) {
		if (num[i] > num[i + 1]) return false;
	}
	return true;
}

long long GetNumOfLessRampNumber(string num)
{
	long long d[80][10];
	for (int i = 0; i < 10; ++i) {
		if (i < num[0] - '0') d[0][i] = 1;
		else d[0][i] = 0;
	}

	for (int i = 1; i < num.size(); ++i) {
		for (int j = 0; j < 10; ++j) {
			d[i][j] = 0;
			for (int k = 0; k <= j; ++k)
				d[i][j] += d[i - 1][k];
			if (j >= num[i - 1] - '0' && j < num[i] - '0')
				++d[i][j];
		}
	}

	long long res = 0;
	for (int i = 0; i < 10; ++i)
		res += d[num.size()-1][i];

	return res;
}
```
