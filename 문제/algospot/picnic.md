# picnic
https://algospot.com/judge/problem/read/PICNIC  
## 풀이
저번에 풀었던 boggle문제에 비하면 금방 풀 수 있는 문제였다.  
(1, 2)와 (2, 1)가 동일한 경우라는 것을 고려해서 풀어야했는데 오름차순으로 탐색하는 것으로 중복문제를 해결했다.
```C++
#include <iostream>
#include <memory.h>

using namespace std;

int C, n, m;
bool friends_list[10][10];
bool taken[10];

int CountPairings(bool taken[10]) {
	int count = 0;
	//짝이 없는 학생 중에서 번호가 빠른 학생을 찾는다
	int first_student = -1;
	for (int i = 0; i < n; i++) {
		if (!taken[i]) {
			first_student = i;
			break;
		}
	}
	//기저조건:짝이 없는 학생이 없으면 1가지 경우 완료
	if (first_student == -1)
		return 1;
	//first_student와 짝이 될 학생을 선정한다
	for (int next_student = first_student + 1; next_student < n; next_student++) {
		if (!taken[next_student] && friends_list[first_student][next_student]) {
			taken[first_student] = true;
			taken[next_student] = true;
			// 쌍이 만들어졌으니 다음쌍 찾는다
			count += CountPairings(taken);
			// 재귀함수 돌고난 후 다음 탐색을 위해 taken 리셋
			taken[first_student] = false;
			taken[next_student] = false;
		}
	}
	return count;
}
int main(void) {
	int y, x;
	cin >> C;
	while (C--) {
		memset(taken, true, sizeof(taken));
		cin >> n >> m;
		for (int i = 0; i < n; i++) {
			taken[i] = false;
		}
		// friends_list 세팅
		for (int i = 0; i < m; i++) {
			cin >> x >> y;
			friends_list[x][y] = true;
			friends_list[y][x] = true;
		}
		int result = CountPairings(taken);
		cout << result << "\n";
		memset(friends_list, false, sizeof(friends_list));
	}
	return 0;
}
```
## 가져가야 할 것
1. memset 함수 사용법
-<string.h>, <memory.h>에 정의됨
-memset(배열 주소, 초기화할 값, 배열이 차지하는 메모리 크기)
2. 순서 상관없이 같은 경우로 취급하는 경우 사전순으로 처리해 
