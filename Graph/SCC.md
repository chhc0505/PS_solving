## Strongly Connected Component (강한 연결 요소)
: 방향 그래프의 SCC는 우선 정점의 최대 부분집합이며, 그 부분집합에 들어있는 서로 다른 임의의 두 정점 u, v에 대해서 u에서 v로 가는 경로와 v에서 u로 가는 경로가 모두 존재하는 경우를 말한다.

  그래프에서 Cycle을 이루는 최대부분집합들을 말한다. (SCC 관련 문제는 양방향 간선이 나올 수 없다. 양방향 간선이라면 Graph 전체 집합이 SCC 하나이기 때문이다.)

- SCC 집합은 한 번의 DFS로 모두 찾을 수 있다. (타잔 알고리즘) 따라서 시간복잡도는 ${O(V+E)}$이다.

0. discovered와 finished 함수 -1 초기화, dfs의 return 값은 현재 정점에서부터 방문할 수 있는 가장 vertex_number가 작은 정점을 return
  
1. SCC_set에 추가하지 않은 정점이 있다면 dfs 시행.
   
2. 정점을 방문했을 때 방문한 순서대로 vertex_number을 부여, stack에 추가
   
3. 주변 이웃들을 방문, 만약 방문하지 않았다면 (discovered[nxt]==-1)이라면 그 정점 방문 후 현 정점에서 방문할 수 있는 가장 작은 verntex_number인 ret 최신화
   만약 방문을 했지만 SCC_set에 추가되지 않은 정점인 경우 (discovered[nxt]>0 && finished[nxt]==-1) 는 부모 또는 조상을 재방문, 즉 Cycle의 생성을 의미. 동일하게 ret 최신화
   
4. 만약 ret (현재 정점에서 갈 수 있는 가장 작은 vertex_number) 가 discovered[node] 라면 현재 정점이 나올 때까지 stack에서 pop하며 SCC 생성

시간복잡도 : ${O(V+E)}$

## 구현
```c++
vector<int> adj[10005];
int discovered[10005], finished[10005];

vector<int> stk;
vector<int> scc_set[10005];
int vertex_number;
```
- adj : 정점의 간방향 간선을 저장하는 배열
- discovered : 정점을 방문했을 때 방문한 순서를 저장하는 배열
- finished : 정점에 SCC 처리를 했는지 정보를 저장하는 배열
- stk : 방문한 정점들을 저장하는 스택 -> SCC 집합을 구하기 위해 필수적이다.
- scc_set : SCC 집합을 저장하기 위한 동적배열
- verntex_number : 방문해야할 정점번호를 저장하는 변수

```c++
int dfs (int node) {
    int ret = discovered[node] = vertex_number++;
    stk.push_back(node);

    for (auto nxt : adj[node]) {
        if (discovered[nxt]==-1) {
            ret = min(ret, dfs(nxt));
        }else if (finished[nxt]==-1) {
            ret = min(ret, discovered[nxt]);
        }
    }

    if (ret==discovered[node]) {
        scc_cnt++;
        while (stk.back()!=node) {
            scc_set[node].push_back(stk.back());
            finished[stk.back()] = node;
            stk.pop_back();
        }
        scc_set[node].push_back(node);
        finished[node] = node;
        stk.pop_back();
    }

    return ret;
}
```
