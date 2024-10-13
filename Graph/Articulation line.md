## 단절선
: 단절선이란 그 간선을 제거했을 때, 그래프가 두 개 또는 그 이상으로 나누어지는 간선을 말한다. 즉, 제거했을 때 그래프의 connected component의 개수가 증가하는 간선을 말한다.

- 부모 p와 자식 c에서 간선 p-c가 절단선이려면 c에서 p나 p의 조상으로 가는 경로가 c->p 말고는 없어야한다.
  
- 구현 방식은 단절점을 구하는 방식과 같다.


## 구현
```c++
#include<bits/stdc++.h>

using namespace std;

int v,e;
vector<int> adj[100005];
int discovered[100005];
vector<pair<int,int>> arti;
int vertex_number;


int dfs (int parent, int node) {
    int ret = discovered[node] = vertex_number++;

    for (auto nxt : adj[node]) {
        if (discovered[nxt]==-1) {
            int tmp = dfs(node, nxt);
            if (tmp>discovered[node]) {
                arti.push_back({node,nxt});
            }
            ret = min(ret, tmp);
        }else if (parent!=nxt) {
            ret = min(ret, discovered[nxt]);
        }
    }
    return ret;
}

int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    cin >> v >> e;

    for (int i=0; i<e; i++) {
        int a,b; cin >> a >> b;
        adj[a].push_back(b);
        adj[b].push_back(a);
    }

    fill(&discovered[0], &discovered[100005], -1);

    for (int i=1; i<=v; i++) {
        if (discovered[i]==-1) {
            dfs(-1,i);
        }
    }

    cout << arti.size() << '\n';
    for (int i=0; i<arti.size(); i++) {
        if (arti[i].first>arti[i].second) {
            int tmp = arti[i].first;
            arti[i].first = arti[i].second;
            arti[i].second = tmp;
        }
    }
    sort(arti.begin(), arti.end());

    for (auto & a : arti) {
        cout << a.first << ' ' << a.second << '\n';
    }

    return 0;
}

```
