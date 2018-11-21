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
3. If \\(a_i\\) is the first flat from the left such that \\(a_{i-2} = 1, a_{i-1} = 0, a_i = 1\\), turning off the light of \\(a_i\\) always yield the optimal solution.

The first assumption is a common mathematical technique to simplify the problem. The second assumption is also obvious since we do not have to turn off the light unless there is a person who is "disturbed and cannot sleep".

The third assumption might be tricky in the first place, but it is also obvious. Among three options, there is a chance to change two people from "disturbed and cannot sleep" to not "disturbed and cannot sleep" at the same time only when \\(a_i\\) is turned off. This case happens when \\(a_{i+1} = 0\\) and \\(a_{i+2} = 1\\).

Now we just traverse the list from left to right and turn off the light based on the assumption 3.

### C. Good Array

Problem: <http://codeforces.com/contest/1077/problem/C>

My code (corrected):

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);

	int n;
	cin >> n;

	vector<pair<int,int> > a(n);
	for (int i = 0; i < n; ++i) {
		pair<int, int> instance;
		cin >> (instance.first);
		instance.second = i+1;
		a[i] = instance;
	}

	sort(a.begin(), a.end());

	//int sum = 0;
	long long sum = 0;
	vector<int> good;

	for (int i = 0; i < n; ++i) sum += (a[i].first);

	for (int i = 0; i < n-1; ++i) {
		if (sum - a[i].first - a[n-1].first == a[n-1].first) {
			good.push_back(a[i].second);
		}
	}
	if (sum - a[n-1].first - a[n-2].first == a[n-2].first) {
		good.push_back(a[n-1].second);
	}

	cout << good.size() << "\n";
	if (good.size() > 0) cout << good[0];
	for (int i = 1; i < good.size(); ++i)
		cout << " " << good[i];
	cout << "\n";

	return 0;
}
```

A simple problem but I was hacked. The algorithm increases \\(i\\) and checks whether removing \\(a_i\\) from the array \\(a\\) results a good array. Each check is \\(O(1)\\), so the whole algorithm runs in \\(O(n)\\).

I was hacked since I did not take into account the overflow of summation. Note that the upper bound for the sum of all \\(a_i\\) is about \\(2 \times 10^{11}\\).

### D. Cutting Out

Problem: <http://codeforces.com/contest/1077/problem/D>

My code (completed after the contest):

```c++
#include <iostream>
#include <vector>
#include <map>
#include <algorithm>
using namespace std;

void find(int lower, int upper, int k, vector<pair<int, int> > counts);
bool check(int num, int k, vector<pair<int, int> > counts);

int main()
{
	ios_base::sync_with_stdio(false);

	int n, k;
	cin >> n >> k;

	map<int, int> hashmap;
	for (int i = 0; i < n; ++i) {
		int val;
		cin >> val;
		++hashmap[val];
	}

	vector<pair<int, int> > counts;

	for (map<int, int>::const_iterator iter = hashmap.begin();
			iter != hashmap.end(); ++iter) {
		pair<int, int> instance;
		instance.first = iter->second;
		instance.second = iter->first;
		counts.push_back(instance);
	}

	sort(counts.begin(), counts.end());
	reverse(counts.begin(), counts.end());

	find(1, n/k+1, k, counts);
	cout << "\n";

	return 0;
}

void find(int lower, int upper, int k, vector<pair<int, int> > counts) {
	if (upper == lower) {
		int i = 0;
		while (k > 0) {
			int repeat = counts[i].first/upper;

			if (k - repeat < 0) 
				for (int j = 0; j < k; ++j) 
					cout << counts[i].second << " ";
			else
				for (int j = 0; j < repeat; ++j) 
					cout << counts[i].second << " ";

			k -= repeat;
			++i;
		}
		return;
	}

	int mid = (upper + lower) / 2 + 1;

	if (check(mid, k, counts))
		find(mid, upper, k, counts);
	else
		find(lower, mid-1, k, counts);
}

bool check(int num, int k, vector<pair<int, int> > counts) {
	int cnt = 0;
	int i = 0;
	while (k > 0) {
		if (i >= counts.size()) break;
		if (counts[i].first < num) break;

		int repeat = counts[i].first/num;
		k -= repeat;
		++i;
	}
	if (k <= 0) return true;
	else return false;
}
```

My algorithm first builds `counts` array that stores the frequency of each integer in the string \\(s\\). It can be done in \\(O(n)\\) if I use hash table, but instead I used binary search tree so it is done in \\(O(n \log n)\\). Then the algorithm sorts `counts` and it also takes \\(O(n \log n)\\).

The final task is to run `find()` function that performs a sort of binary search on the number of copies of array \\(t\\) from array \\(s\\). This function is called \\(O(\log (n/k))\\) times, and each call takes \\(O(n)\\) since it goes through the array `counts`.

As a result, the time complexity of my algorithm is \\(O(n \log n)\\). Different approach is demonstrated on the following link.

<http://codeforces.com/blog/entry/63274>
