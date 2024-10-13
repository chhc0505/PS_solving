## 단절점이란 ?
: 단절점이란 그 정점을 제거했을 때, 그래프가 두 개 또는 그 이상으로 나누어지는 정점을 말한다. 즉, 제거했을 때 그래프의 connected component의 개수가 증가하는 정점을 말한다.

- 루트 노드는 자식의 개수가 2개 이상이라면 단절점이다. 1개 이하라면 단절점이 아니다. (자식의 개수는 루트 노드의 이웃노드의 개수를 의미하는게 아니다!)
- 루트 노드가 아닌 노드의 단절점 판단규칙은 다음과 같다.
  : 현재 방문한 노드를 u라고 했을 때 u의 자식노드가 u의 부모 또는 조상을 방문할 수 있으면 u는 단절점이 아니다.
  : 만약 하나의 자식이라도 조상노드를 방문할 수 없으면 u는 단절점이다.

## 구현

```c++
#include<bits/stdc++.h>


using namespace std;

int v,e, ROOT;
vector<int> adj[100005];
int discovered[100005];
bool arti[100005];
int vertex_number;

int dfs (int parent, int node) {
    int ret = discovered[node] = vertex_number++;
    int sub(0);
    for (auto nxt : adj[node]) {
        if (discovered[nxt]==-1) {
            int tmp = dfs(node, nxt);
            sub++;
            if (node!=ROOT && tmp>=discovered[node])
                arti[node] = true;
            ret = min(ret, tmp);
        }else if (nxt!=parent){
            ret = min(ret, discovered[nxt]);
        }
    }
    if (node==ROOT) {
        if (sub>=2)
            arti[node] = true;
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
            ROOT = i;
            dfs(-1,i);
        }
    }

    vector<int> ans;
    for (int i=1; i<=v; i++) {
        if (arti[i]) {
            ans.push_back(i);
        }
    }

    cout << ans.size() << '\n';
    sort(ans.begin(), ans.end());
    for (auto a : ans) {
        cout << a << ' ';
    }
    cout << '\n';

    return 0;
}

```
