## Trie 알고리즘 ?
- 문자열과 같은 일련의 sequence 들을 저장하고 효율적으로 탐색하기 위해 사용하는 알고리즘
- 문자열을 ${O(n)}$ 의 시간복잡도로 삽입(insert), 탐색(find), 삭제(erase)가 가능하지만 공간복잡도가 ${O(4*MX)}$이다.
- sequence를 저장하는데 트리 자료구조를 사용

## 초기화
Trie 알고리즘 구현을 위한 초기화는 다음과 같다.

```c++
int ROOT = 1;
int unused = 2;
const int MX = 1e5+5;

bool chk[MX];
int nxt[MX][26];
vector<pair<string, int>> nxt[MX]
unordered_map<string,int> nxt[MX]
```
- ROOT : 트리의 루트번호를 가리킴
- unused : 다음으로 사용할 노드 번호를 저장
- MX : 노드로 삽입해야하는 최대 갯수
- chk : 노드가 특정 sequence의 마지막 노드인지 저장하는 배열
- nxt : 다음 노드의 번호를 저장

```c++
void insert (string & s) {
  int cur = ROOT;
  for (auto c : s) {
    if (nxt[cur][c-'A']==-1) {
      nxt[cur][c-'A'] = unused;
      unused++;
    }
    cur = nxt[cur][c-'A'];
  }
  chk[cur] = true;
}
```
: Trie에 노드를 삽입하는 insert 함수

```c++
bool find (string & s) {
  int cur = ROOT;
  for (auto c : s) {
    if (nxt[cur][c-'A']==-1) {
      return false;
    }
    cur = nxt[cur][c-'A'];
  }
  return chk[cur];
}
```
: Trie에서 sequece s를 찾는 find 함수

```c++
void erase (string & s) {
  int cur = ROOT;
  for (auto c : s) {
    if (nxt[cur][c-'A']==-1) {
      return;
    }
    cur = nxt[cur][c-'A'];
  }
  chk[cur] = false;
}
```
: Trie에서 sequce s를 삭제하는 erase 함수

  

