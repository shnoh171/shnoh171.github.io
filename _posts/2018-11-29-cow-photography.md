---
layout: post
title: Cow Photography (USACO December 2012 Bronze Div.)
categories:
  - Competitive Programming
---

Problem: <http://www.usaco.org/index.php?page=viewproblem2&cpid=95>

My code:

```c++
#include <iostream>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

bool IsRange(int i, int n) { return i >= 0 && i < n; }
bool IsAhead(int front, int back, vector<vector<int> >& cow_position);

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int n;
	cin >> n;

	int cow_sequence[5][n];
	vector<vector<int> > cow_position(5, vector<int>(n+1));

	for (int i = 0; i < 5; ++i) {
		for (int j = 0; j < n; ++j) {
			int cow;
			cin >> cow;
			cow_sequence[i][j] = cow;
			cow_position[i][cow] = j;
		}
	}

	int res[n];
	int prev_cow = 0, preprev_cow = 0;

	for (int i = 0; i < n; ++i) {
		map<int, int> counts;

		for (int j = i-1; j <= i+1; ++j) {
			if (!IsRange(j, n)) continue;
			for (int k = 0; k < 5; ++k) {
				int cow = cow_sequence[k][j];
				if (cow != prev_cow && cow != preprev_cow)
					++counts[cow];
			}
		}

		vector<int> candidates;
		for (map<int, int>::const_iterator k = counts.begin(); k != counts.end(); ++k) 
			if (k->second >= 4) candidates.push_back(k->first);

		int idx = 0;
		for (int j = 1; j < candidates.size(); ++j) 
			if (IsAhead(candidates[j], candidates[idx], cow_position)) 
				idx = j;

		res[i] = candidates[idx];
		preprev_cow = prev_cow;
		prev_cow = res[i];
	}

	for (int i = 0; i < n; ++i )
		cout << res[i] << '\n';

	return 0;
}

bool IsAhead(int front, int back, vector<vector<int> >& cow_position)
{
	int count = 0;
	for (int i = 0; i < 5; ++i)
		if (cow_position[i][front] < cow_position[i][back])
			++count;

	return count >= 3;
}
```

Let \\( a^1_1,   \\)

