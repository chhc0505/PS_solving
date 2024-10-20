## 이분 그래프
그래프의 정점 집합을 둘로 나눴을 때 각 집합에 속한 정점들 끼리는 서로 인접하지 않도록 분할할 수 있는 그래프를 말한다.

## 이분 매칭
이분그래프에서 정점집합 A,B가 있다고 가정하자.

source와 sink 정점을 추가하는데 source에는 정점집합 A와 연결하고 B는 sink와 연결하자.

모든 간선의 가중치(Capacity)는 1일 때, source에서 sink로 흐르는 최대유량을 구하는 알고리즘을 이분매칭이라한다.

최대유량을 이용해서 이분매칭을 ${O(V*E^2)}$에 해결할 수 있다.

하지만 DFS를 이용하면 ${O(V*E)}$에 해결할 수 있다.

## 구현
```c++
#include<bits/stdc++.h>


using namespace std;

int match;
int n = 1000;
int A[1005],B[1005];
bool visited[1005];
vector<int> adj[1005];

bool dfs (int a) {
    visited[a] = true;
    for (int b : adj[a]) {
        if (B[b]==-1 || !visited[B[b]] && dfs(B[b])) {
            A[a] = b;
            B[b] = a;
            return true;
        }
    }

    return false;
}
int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    fill(&A[0], &A[1005], -1);
    fill(&B[0], &B[1005], -1);

    for (int i=0; i<n; i++) {
        if (A[i]==-1) {
            fill(&visited[0], &visited[1005], false);
            if (dfs(i)) match++;
        }
    }

    cout << match << '\n';

    return 0;
}
```
한번의 dfs로 최대 새로운 노드 1개의 매칭을 성사시킨다.

visited 배열을 노드에서 현재 성사된 매칭을 바꿀 수 없을 때 유효하다.

이분그래프에서 간선은 되도록 간방향 간선으로 추가해주자. 만약 양방향으로 추가한다면 집합 A와 집합 B의 idx가 겹치면 안된다.

## Minimum Vertex Cover

Vertex Cover는 다음과 같은 특성을 만족시키는 그래프 정점의 부분집합이다.

- 모든 간선들의 양 끝 정점 중 하나 이상이 Vertex Cover이다.

Vertex Cover 중 최소의 집합을 Minimum Vertex Cover 라고 한다.

Minimum Vertex Cover는 이분 매칭을 이용해서 구할 수 있다. 즉 Minimum Vertex Cover = maximum matching 이다. 이는 콰닉의 정리를 이용해 증명할 수 있다. (직관적으로도 이해가 간다.)

Minimum Vertex Cover을 구하면 최대독립집합(간선으로 연결되지 않은 정점의 수!!! 정점 u에서 정점 v로 갈 수 없다!)을 구할 수 있다. 


