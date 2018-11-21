---
layout: post
title: Making Code Simple
categories:
  - Competitive Programming
---

After participating in the coding contest for two weeks, I realized that the one of the best ways to increase the speed and accuracy of implementation is to simplify the code. In many cases, my code was longer and more complex compared to top competitors even the same algorithm was implemented.

```c++
#include <iostream>
using namespace std;

bool hasWord(int y, int x, string word, char board[5][5]);

int main()
{
	ios_base::sync_with_stdio(false);

	int c;
	cin >> c;

	for (int i = 0; i < c; ++i) {
		char board[5][5];
		for (int j = 0; j < 5; ++j) {
			string s;
			cin >> s;
			for (int k = 0; k < 5; ++k)
				board[j][k] = s[k];
		}

		int n;
		cin >> n;

		for (int j = 0; j < n; ++j) {
			string word;
			bool exist = false;
			cin >> word;

			for (int k = 0; k < 5; ++k) {
				for (int l = 0; l < 5; ++l) {
					exist = hasWord(k, l, word, board);
					if (exist) break;
				}
				if (exist) break;
			}

			cout << word << " ";
			cout << ((exist) ? "YES" : "NO") << "\n";
		}
	}

	return 0;
}

bool hasWord(int y, int x, string word, char board[5][5]) {
	if (word.empty()) return true;

	if (word[0] == board[y][x]) {
		bool ret = false;
		for (int i = -1; i <= 1; ++i) {
			for (int j = -1; j <= 1; ++j) {
				if (i == 0 && j == 0) continue;
				if (y+i < 0 || y+i >= 5 || x+j < 0 || x+j >= 5) continue;
				ret = hasWord(y+i, x+j, word.substr(1, word.size()-1), board);
				if (ret) break;
			}
			if (ret) break;
		}
		return ret;
	} else {
		return false;
	}
}
```

The above code is an implementation of a simple recursive function that solves the following problem in brute force manner. (Note that this is not optimal algorithm)

<https://www.geeksforgeeks.org/boggle-find-possible-words-board-characters/>

/* Writing */


