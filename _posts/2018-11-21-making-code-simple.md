---
layout: post
title: Making Code Simple
categories:
  - Problem Solving
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

However, this code is not easy to read (due to various reasons). From now on, I will try to simplify the code.

```c++
struct Delta {
	int dy;
	int dx;
};

const Delta deltas[8] = {
	{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0,1}, {1,-1}, {1,0}, {1,1}
};

/* skip main() function */

bool hasWord(int y, int x, string word, char board[5][5]) {
	if (word.empty()) return true;

	if (word[0] == board[y][x]) {
		bool ret = false;

		for (int i = 0; i < 8; ++i) {
			int dy = deltas[i].dy;
			int dx = deltas[i].dx;

			if (y+dy < 0 || y+dy >= 5 || x+dx < 0 || x+dx >= 5) continue;
			ret = hasWord(y+dy, x+dx, word.substr(1, word.size()-1), board);
			if (ret) break;
		}
		return ret;
	} else {
		return false;
	}
}
```

First, I removed nested loop in `hasWord()` by expressing the direction of movement in an array of `struct Delta`. This improves the readability of the function `hasWord()`.

```c++
bool isRange(int y, int x) { return  x < 0 || x >= 5 || y < 0 || y >= 5; }

/* skip */

bool hasWord(int y, int x, string word, char board[5][5]) {
	if (word.empty()) return true;

	if (word[0] == board[y][x]) {
		bool ret = false;

		for (int i = 0; i < 8; ++i) {
			int dy = deltas[i].dy;
			int dx = deltas[i].dx;

			if (isRange(y+dy, x+dx)) continue;
			ret = hasWord(y+dy, x+dx, word.substr(1, word.size()-1), board);
			if (ret) break;
		}
		return ret;
	} else {
		return false;
	}
}
```

Second, I introduced a simple function named `isRange()`. By doing so, complex contional statement in `hasWord()` is removed.

```c++
bool hasWord(int y, int x, string word, char board[5][5]) {
	if (isRange(y, x)) return false;
	if (word.empty()) return true;
	if (word[0] != board[y][x]) return false;

	bool ret = false;

	for (int i = 0; i < 8; ++i) {
		int dy = deltas[i].dy;
		int dx = deltas[i].dx;

		ret = hasWord(y+dy, x+dx, word.substr(1, word.size()-1), board);
		if (ret) break;
	}
	return ret;
}
```

Third I modifed the base cases of `hasWord()`. Since the checking range is now done in the beginning of the function, all the conditional statements after base cases are removed.

```c++
bool hasWord(int y, int x, string word, char board[5][5]) {
	if (isRange(y, x)) return false;
	if (word.empty()) return true;
	if (word[0] != board[y][x]) return false;

	bool ret = false;

	for (int i = 0; i < 8; ++i) {
		int dy = deltas[i].dy;
		int dx = deltas[i].dx;

		if (hasWord(y+dy, x+dx, word.substr(1, word.size()-1), board))
			return true;
	}

	return false;
}
```

Now the code is simplified enough for me to notice that `break` statement is used in a strange manner. It is much more intuitive to return `true` immediately when we find the target word.

```c++
bool hasWord(int y, int x, string word, char board[5][5]) {
	if (isRange(y, x)) return false;
	if (word[0] != board[y][x]) return false;
	if (word.size() == 1) return true;

	for (int i = 0; i < 8; ++i) {
		int dy = deltas[i].dy;
		int dx = deltas[i].dx;

		if (hasWord(y+dy, x+dx, word.substr(1), board))
			return true;
	}

	return false;
}
```

Two more modification and the code is complete.

- I reduced the number of function calls by returning `true` when `word.size() == 1` and it has a correct letter.
- I used `substr()` method of `word` more neatly.

The whole result is in the following link.

<https://github.com/shnoh171/problem-solving/blob/master/algospot/boggle/boggle_bf_enhanced.cpp>
