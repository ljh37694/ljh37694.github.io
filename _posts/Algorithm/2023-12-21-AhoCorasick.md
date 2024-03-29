---
title: "Aho-Corasick: 문자열 알고리즘"
comments: true
toc: true
toc_sticky: true
toc_label: "On this page"
categories:
  - Algorithm
tags:
  - Algorithm
  - Trie
  - String
  - KMP
  - BFS
---

<br>

## 아호코라식이란?
일대일 문자열 패턴 매칭인 KMP의 확장으로, 하나의 문자열에서 여러 패턴을 찾는 일대다 패턴 매칭 알고리즘이다.  
[Trie](https://ljh37694.github.io/algorithm/Trie/)와 [KMP](https://ljh37694.github.io/algorithm/KMP/)를 사용해 구현한다.

<br>

## 구현

[9250: 문자열 집합 판별](https://www.acmicpc.net/problem/9250)의 풀이로 진행한다.  

`Trie`와 `KMP`가 선행되었다고 가정한다.

<br>

**1. 트라이에 패턴 삽입**  
트라이에 찾고자 하는 패턴을 삽입한다.

```c++
void AhoCorasick::insert(string s) {
	Node* cur = root;

	for (char c : s) {
		int idx = c - 'a';

		if (cur->children[idx] == nullptr) {
			cur->children[idx] = new Node;
		}

		cur = cur->children[idx];
	}

	cur->isEnd = true;
}
```

<br>

**2. failure 함수**

`bfs`로 다음 문자열을 찾지 못했을 때 이동할 곳을 찾는 `failure` 함수를 만든다.

```c++
void AhoCorasick::failure() {
	queue<Node*> q;

	root->fail = root;

	q.push(root);

	while (!q.empty()) {
		Node* cur = q.front();
		q.pop();

		for (int i = 0; i < 26; i++) {
			Node* next = cur->children[i];

			if (next == nullptr) continue;

			if (cur == root) {
				next->fail = root;
			}

			else {
				Node* fail = cur->fail;
				while (fail != root && fail->children[i] == nullptr) {
					fail = fail->fail;
				}

				if (fail->children[i]) {
					fail = fail->children[i];
				}

				next->fail = fail;

				if (next->fail->isEnd) {
					next->isEnd = true;
				}
			}
			q.push(next);
		}
	}
}
```

> `kmp`와 비슷한 방식이다.

<br>

**3. search**
```c++
bool AhoCorasick::search(string text) {
	Node* cur = root;

	for (char c : text) {
		int idx = c - 'a';

		while (cur != root && cur->children[idx] == nullptr) {
			cur = cur->fail;
		}

		if (cur->children[idx]) {
			cur = cur->children[idx];
		}

		if (cur->isEnd) {
			return true;
		}
	}

	return false;
}
```

<br>

## 전체 코드
```c++
#include <iostream>
#include <queue>

using namespace std;

struct Node {
	Node* children[26];
	Node* fail;
	bool isEnd;

	Node() : fail(nullptr), isEnd(false) {
		for (int i = 0; i < 26; i++) {
			children[i] = nullptr;
		}
	}

	~Node() {
		for (int i = 0; i < 26; i++) {
			if (children[i]) {
				delete children[i];
			}
		}
	}
};

class AhoCorasick {
private:
	int n, m;
	vector<string> patterns;
	vector<string> texts;
	Node* root;

	void insert(string s);

	void failure();

	bool search(string ptn);

	void solution();

public:
	AhoCorasick();
};

AhoCorasick::AhoCorasick() {
	cin >> n;
	for (int i = 0; i < n; i++) {
		string s; cin >> s;
		patterns.push_back(s);
	}

	cin >> m;
	for (int i = 0; i < m; i++) {
		string s; cin >> s;
		texts.push_back(s);
	}

	root = new Node;

	solution();

	delete root;
}

void AhoCorasick::insert(string s) {
	Node* cur = root;

	for (char c : s) {
		int idx = c - 'a';

		if (cur->children[idx] == nullptr) {
			cur->children[idx] = new Node;
		}

		cur = cur->children[idx];
	}

	cur->isEnd = true;
}

void AhoCorasick::failure() {
	queue<Node*> q;

	root->fail = root;

	q.push(root);

	while (!q.empty()) {
		Node* cur = q.front();
		q.pop();

		for (int i = 0; i < 26; i++) {
			Node* next = cur->children[i];

			if (next == nullptr) continue;

			if (cur == root) {
				next->fail = root;
			}

			else {
				Node* fail = cur->fail;
				while (fail != root && fail->children[i] == nullptr) {
					fail = fail->fail;
				}

				if (fail->children[i]) {
					fail = fail->children[i];
				}

				next->fail = fail;

				if (next->fail->isEnd) {
					next->isEnd = true;
				}
			}
			q.push(next);
		}
	}
}

bool AhoCorasick::search(string text) {
	Node* cur = root;

	for (char c : text) {
		int idx = c - 'a';

		while (cur != root && cur->children[idx] == nullptr) {
			cur = cur->fail;
		}

		if (cur->children[idx]) {
			cur = cur->children[idx];
		}

		if (cur->isEnd) {
			return true;
		}
	}

	return false;
}

void AhoCorasick::solution() {
	for (string ptn : patterns) {
		insert(ptn);
	}

	failure();

	for (string text : texts) {
		cout << (search(text) ? "YES" : "NO") << "\n";
	}
}

int main(void) {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	AhoCorasick ac;
}
```

<br>

## 문제
[9250: 문자열 집합 판별](https://www.acmicpc.net/problem/9250)  
[10256: 돌연변이](https://www.acmicpc.net/problem/10256)

<br>

## 느낀점
개인적으로 아호코라식이 kmp보다 더 이해하기 쉬웠다. 그동안 공부했던 `bfs`, `trie`, `kmp`를 모두 사용해서 문제를 푸니까 예전에 비해서 많이 성장했다는 것을 느꼈다.

<br>