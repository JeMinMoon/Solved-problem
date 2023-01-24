# boggle
https://www.algospot.com/judge/problem/read/BOGGLE
## 풀이
시간초과를 해결하기 위해서는 memorization(메모이제이션)을 사용했어야 해결되는 문제였다.  
탐색에 필요한 상대위치를 따로 배열로 저장해서 반복문을 사용하는 테크닉을 배울 수 있었다.  
메모이제이션 활용이 미숙해서 실수를 했다.  
메모이제이션을 효과적으로 이용하려면 반복되는 작업을 최소화해야 하는데 재귀함수로 끝까지 탐색한 
결과가 아닌 글자 하나만을 비교해서 cache에 저장하는 바람에 탐색시간을 효과적으로 줄이지 못했다.

```C++
#include <iostream>
#include <string>
#include<memory.h> 

using namespace std;

const int dx[8] = { -1,-1,-1,0,0,1,1,1 };
const int dy[8] = { -1,0,1,-1,1,-1,0,1 };
int cache[10][5][5];
string board[5];
string word;

bool hasWord(int y, int x, int word_index) {	
	//범위 밖이면 실패
	if (y < 0 || y>4 || x < 0 || x>4)
		return false;
	//글자 일치하지 않으면 실패
	if (cache[word_index][y][x] != true) {
		if (cache[word_index][y][x] == false)
			return false;
		else if (board[y][x] != word[word_index]) {
			cache[word_index][y][x] = false;
			return false;
		}
	}
	//캐시값 true이면 탐색 중지하고 true리턴
	if (cache[word_index][y][x] == true) {
		return true;
	}
	//마지막 글자이면 true
	if (word.size() - 1 == word_index)
		return true;
	//다음글자 탐색
	for (int direction = 0; direction < 8; ++direction) {
		int next_y = y + dy[direction], next_x = x + dx[direction];
		int next_index = word_index + 1;
		if (hasWord(next_y, next_x, next_index)) {
			cache[word_index][y][x] = true;
			return true;
		}
		else
			cache[word_index][y][x] = false;
	}
	return false;
}

int main() {
	memset(cache, -1, sizeof(cache));
	int testcase;
	cin >> testcase;
	cin.ignore();
	for (int i = 0; i < testcase; ++i) {
		
		for (int j = 0; j < 5; ++j) {
			getline(cin, board[j]);
		}
		int number_of_words;
		cin >> number_of_words;
		cin.ignore();
		for (int j = 0; j < number_of_words; ++j) {
			getline(cin, word);
			//모든 좌표 확인
			for (int a = 0; a < 5; ++a) {
				for (int b = 0; b < 5; ++b) {
					if (hasWord(a, b, 0) == true) {
						cout << word << " YES\n";
						a = 10;
						break;
					}
				}
				//단어를 찾지 못하는 경우 처리
				if (a>=4 && a != 10) { 
					cout << word << " NO\n";
				}
			}
			//다음 단어 처리전에 cache와 word초기화
			memset(cache, -1, sizeof(cache));
			word.clear();
		}
	}
	return 0;
}
```
## 이 문제에서 가져가야 할 것
1. memset함수 활용해서 배열 초기화할 수 있다. (<string.h>와 <memory.h>에 정의됨)  
void* memset(void* ptr, int value, size_t num);  
(배열 시작주소, 세팅하려는 값, 배열이 차지하는 메모리 크기)  
메모리 크기 = 배열 크기 * sizeof(자료형) 
or 메모리 크기 = sizeof(배열이름)
2. 인접 위치 탐색시 상대위치(x,y좌표)를 따로 배열에 저장해서 반복문에서 배열 인덱스로 비교하면 편리하다.
3. 같은 값을 반복해서 구하는 작업이 있다면 memorization(메모이제이션)을 고려하자.
