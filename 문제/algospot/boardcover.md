# boardcover
https://algospot.com/judge/problem/read/BOARDCOVER
## 풀이
- 2차원 배열의 주소를 함수에 어떻게 넘겨야 되는지 헷갈려서 구글링해서 풀었다. 
- 재귀함수가 돌다가 잘못된 경우일 때 변경했던 값들을 원상복구해주어야 됐는데 원상복구를 어떻게 해줘야 할지 헤맸다.

```C++
#include <iostream>
#include <memory.h>
#include <string>
using namespace std;

int C, H, W;
int white_count = 0;//흰색 개수
int board[20][20];
const int cover_type[4][3][2] = {
	{{0,0},{0,1},{1,0}},
	{{0,0},{0,1},{1,1}},
	{{0,0},{1,0},{1,1}},
	{{0,0},{1,-1},{1,0}}
};

bool CanCover(int (& board)[20][20], int y, int x, int type, int fnc) {//fnc 1이면 탐색 후 board[ny][nx] 덮은 걸로 처리, -1이면 원상복구
	bool ret = true;
	for (int i = 0; i < 3; i++) {
		int ny = y + cover_type[type][i][0];
		int nx = x + cover_type[type][i][1];
		if (ny < 0 || ny >= H || nx < 0 || nx >= W) { //범위 벗어난 경우
			ret = false;
		}
		else if ((board[ny][nx] += fnc) > 1) { // 빈칸이 아닌 경우
			board[ny][nx] += fnc;
			ret =  false;
		}
	}
	return ret;
}

int Cover(int (& board)[20][20]) {
	int x = -1;
	int y = -1;
	int count = 0;
	//흰칸 3의 배수 아니면 답 없음
	if (white_count % 3 != 0)
		return 0;
	//채우지 않은 칸 중 가장 좌상단에 있는 칸 찾음
	for (int i = 0; i < H; i++) {
		for (int j = 0; j < W; j++) {
			if (board[i][j] == 0) {
				y = i;
				x = j;
				break;
			}
		}
		if (y != -1) break;
	}

	//기저사례:모든 칸 채웠으면 1반환
	if (y == -1) 
		return 1;
	for (int type = 0; type < 4; type++) {
		if (CanCover(board, y, x, type, 1)) {
			count += Cover(board);
			CanCover(board, y, x, type, -1);
		}
		else {
			CanCover(board, y, x, type, -1);
		}
	}
	return count;
}

int main(void) {
	//검은색 or 채운 칸:1, 빈칸:0
	
	char color;
	cin >> C;
	while (C--) {
		cin >> H >> W;
		for (int i=0; i < H; i++) {
			for (int j=0; j < W; j++) {
				cin >> color;
				if (color == '#') {//검은색일 경우
					board[i][j] = 1;
				}
				else if (color == '.') {//흰색일 경우
					board[i][j] = 0;
					white_count++;
				}
			}
		}
		cout << Cover(board) << '\n';
		white_count = 0;
		memset(board, 0, sizeof(board));
	}

	return 0;
}
``` 
## 가져갈 것
1. 이차원 배열을 함수의 매개변수로 받을 때 어떻게 써야 하는지 숙지하자.
 ```C++
 int (*board)[20]
 ```
 ```C++
 int board[][20]
 ```
 ```C++
 int (& board)[20][20]
 ```
 ```C++
 int board[20][20]
 ```
2. 반복이 끝날 때 변수 초기화 잊지 말자.
