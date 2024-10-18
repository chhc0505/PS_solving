## 최대 유량
두 정점 사이에서 얼마나 많은 유량을 보낼 수 있는지 계산하는 알고리즘이다. 양방향 그래프에서는 역간선도 만들어줘야한다. 

- 용량 (Capacity) : c[u][v] 로 정점 u에서 정점 v로 가는 간선의 용량(가중치)이다.

- 유량 (Flow) :  f[u][v] 로 정점 u에서 정점 v로 현재 흐르고 있는 유량을 말한다.

- 잔여 용량 (Residual Capacity) : 간선의 용량과 유량의 차이를 의미한다. c[u][v] - f[u][v], 해당 간선에 추가적으로 흘려 보낼 수 있는 유량을 의미한다.

- 소스 (Source) : 유량이 시작되는 정점으로 모든 유량은  Source에서 sink로 흐른다.

- 싱크 (Sink) : 모든 유량이 도착하는 정점이다.

## 포드 풀거슨 알고리즘
: 모든 간선의 유량을 0으로 시작해서 소스에서 싱크로 유량을 흘려보낼 수 있는 모든 증가경로를 찾아 최대한의 유량을 흘려보내는 것을 반복한다.

증가경로를 찾았을 때 흘려보낼 수 있는 최대의 유량은 증가경로 상에 있는 간선들 중 가장 용량 (Capacity) 값이 낮은 간선의 용량 amount 만큼이다.

미리 역추적을 위해 부모 노드를 저장한 parent 배열을 이용해 경로에 있는 간선들의 유량을 늘리고 그 간선들의 역은 줄인다. --> 유량의 대칭성 : u,v로 f의 유량이 흐르면 v,u로는 -f의 유량이 흐른다.

이를 더이상의 증가경로가 존재하지 않을 때까지 반복한다.


## 에드몬드-카프 알고리즘
: 포드 풀거슨 알고리즘을 BFS를 이용해 구현한 알고리즘이다. (DFS로도 가능하지만 복잡도가 용량값에 따라 달라질 수 있다.)

시간복잡도 : ${O(E^2*V)}$

증명 : https://kasterra.github.io/flowGraph2/

```c++
#include<bits/stdc++.h>

using namespace std;

const int INF = 2e9;
int c[1005][1005];
int f[1005][1005];
vector<int> adj[1005];

int maximumFlow (int src, int sink) {
    int total_flow(0);

    while (true) {
        int parent[1005];
        queue<int> q;
        fill(&parent[0], &parent[1005], -1);
        parent[src] = src;
        q.push(src);

        while (!q.empty() && parent[sink]==-1) {
            int curr = q.front(); q.pop();
            for (auto nxt : adj[curr]) {
                if (parent[nxt]==-1 && c[curr][nxt]-f[curr][nxt]>0) {
                    q.push(nxt);
                    parent[nxt] = curr;
                }
            }
        }

        if (parent[sink]==-1) {
            break;
        }

        int amount = INF;
        for (int p=sink; p!=src; p=parent[p]) {
            amount = min(amount, c[parent[p]][p]-f[parent[p]][p]);
        }
        
        for (int p=sink; p!=src; p=parent[p]) {
            f[parent[p]][p] += amount;
            f[p][parent[p]] -= amount;
        }

        total_flow += amount;
    }

    return total_flow;
}



int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    // input c, adj

    return 0;
}
```
