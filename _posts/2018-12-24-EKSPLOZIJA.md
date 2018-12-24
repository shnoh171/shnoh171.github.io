---
layout: post
title: EKSPLOZIJA (COCI 2013/2014 Contest \\#5)
categories:
  - Competitive Programming
---

Problem: <http://hsin.hr/coci/archive/2013_2014/contest5_tasks.pdf>

Online Judge: <https://www.acmicpc.net/problem/9935> (Select English from the right)

The most important thing to notice is that the order of explosion never change the result, since the explosion does not contain any two equal chracters. Let Mirko's string \\(S\\) and \\(s_i\\) denotes the \\(i\\)th character of of \\(S\\). Also, let the explosion string \\(E\\) and \\(e_i\\) denotes the \\(i\\)th character of of \\(E\\).

All I have to do is traverse \\(S\\) from \\(i=1\\) to \\(m\\) (where \\(m\\) is the size of \\(S\\)) and remove a part of the \\(S\\) if it is equal to \\(E\\). The implementation becomes much simpler if we use stack. For each \\(s_i\\), I push \\(s_i\\) into the stack and check whether the top \\(n\\) characters are equal to \\(E\\) (where \\(n\\) is the size of \\(E\\)). If so, we pop \\(n\\) characters and continue.

If abovementioned algorithm is implemented naively, the time complexity of the algorithm is \\(O(mn)\\). However, a little trick can be added to the elements of the stack and it becomes \\(O(m)\\). The key idea is to push the currently matched index \\(j\\) of \\(e_j\\) with \\(s_i\\). Then the time to check the top \\(n\\) characters becomes \\(O(1)\\) instead of \\(O(n)\\).

The code is as follows.

```c++
#include <iostream>
#include <stack>
#include <string>
using namespace std;

void PrintNotEmptyStack(stack<pair<char, int>> &s);

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	string text, explosion;
	cin >> text >> explosion;

	stack<pair<char, int>> s;
	for (int i = 0; i < text.size(); ++i) {
		if (s.empty() || s.top().second == -1) {
			if (text[i] == explosion[0])
				s.push(pair<char, int>(text[i], 0));
			else
				s.push(pair<char, int>(text[i], -1));
		}
		else {
			int prev = s.top().second;
			if (text[i] == explosion[prev+1])
				s.push(pair<char, int>(text[i], prev + 1));
			else if (text[i] == explosion[0])
				s.push(pair<char, int>(text[i], 0));
			else
				s.push(pair<char, int>(text[i], -1));
		}

		if (s.top().second == explosion.size() - 1) {
			for (int j = 0; j < explosion.size(); ++j)
				s.pop();
		}
	}

	if (s.empty()) {
		cout << "FRULA\n";
	}
	else {
		PrintNotEmptyStack(s);
		cout << "\n";
	}

	return 0;
}

void PrintNotEmptyStack(stack<pair<char, int>> &s)
{
	if (s.empty()) return;

	char c = s.top().first;
	s.pop();
	PrintNotEmptyStack(s);
	cout << c;

	return;
}
```
