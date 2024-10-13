## LCA ?
LCA는 최소 공통 조상으로 트리에서 정점 u와 v가 있을 때, u와 v에 가장 가까운 조상을 구하는 것이다.
시간복잡도는 DFS를 사용하는데 ${O(V+E)}$, LCA를 구하는데 ${O(트리의 깊이)}$ 이다.

## 구현
1. DFS를 이용해서 parent 배열과 depth 배열을 채운다.

2. 두 정점의 깊이를 같게 만든다.

3. 두 정점의 부모를 재귀적으로 확인하고 부모가 같아지면 그 정점이 LCA다.


```c++
#include<bits/stdc++.h>

using namespace std;

int n,m;
vector<int> adj[50005];
int parent[50005];
int depth[50005];

void dfs (int node) {
    for (auto nxt : adj[node]) {
        if (parent[node]==nxt) continue;
        parent[nxt] = node;
        depth[nxt] = depth[node]+1;
        dfs(nxt);
    }
}

int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n;
    for (int i=0; i<n-1; i++) {
        int u,v; cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    cin >> m;
    dfs(1);
    for (int i=0; i<m; i++) {
        int u,v; cin >> u >> v;
        int same_depth_cnt = abs(depth[u]-depth[v]);
        if (depth[u]>depth[v]) {
            while (same_depth_cnt--)
                u = parent[u];
        }else if (depth[u]<depth[v]) {
            while (same_depth_cnt--)
                v = parent[v];
        }
        
        while (u!=v) {
            u = parent[u];
            v = parent[v];
        }

        cout << u << '\n';
    }

    return 0;
}
```

   
