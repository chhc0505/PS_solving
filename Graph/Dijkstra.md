## Dijkstra
: 가중치가 양수인 그래프에서 한 정점에서 나머지 정점까지의 최단거리를 구하는 알고리즘

## 구현
- 매 단계마다 남아있는 정점 중 시작점으로부터 거리가 가장 가까운 정점을 구하, 시작점으로부터 해당 정점까지의 거리를 확정하는 방식 !

- 시간복잡도 : ${O(V^2+E)}$ or ${O(ElogE)}$

```c++
#include<bits/stdc++.h>

using namespace std;

const int INF = 1e9+5;
int v,e;
vector<pair<int,int>> adj[1005];
vector<int> path;
int d[1005], pre[1005];

void dijkstra (int st, int en) {
    fill(&d[0], &d[1005], INF);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
    d[st] = 0;
    pq.push({d[st], st});

    while (!pq.empty()) {
        auto curr = pq.top(); pq.pop();
        if (d[curr.second]!=curr.first) continue;
        for (auto nxt : adj[curr.second]) {
            if (d[nxt.second]>d[curr.second]+nxt.first) {
                d[nxt.second] = d[curr.second]+nxt.first;
                pq.push({d[nxt.second], nxt.second});
                pre[nxt.second] = curr.second;
            }
        }
    }

    int curr = en;
    while (curr!=st) {
        path.push_back(curr);
        curr = pre[curr];
    }

    path.push_back(curr);
    reverse(path.begin(), path.end());
    return;
}

int main () {

    ios::sync_with_stdio(0); cin.tie(0);

    // 간선 정보 입력

    dijkstra();

    return 0;
}


```
