## Union Find

공통 원소가 없는 상호 베타적인 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 관리하는 자료구조이다. 

Union_Find의 연산으로는 대표적으로 2가지가 있다.

1. Union : 두 원소 a,b가 주어질 때 이들이 속한 두 집합을 하나의 집합으로 합친다.
2. Find : 어떤 원소 a가 주어질 때 a가 속한 집합을 반환


## 어떻게 구현할까 ?

배열과 대표자(트리의 root)의 개념을 이용해서 구현한다. 다음 코드가 Union Find의 가장 기본적인 구현 코드이다.
```c++
#include<bits/stdc++.h>

using namespace std;

int n;
int parent[11];

int uFind (int u) {
    if (u==parent[u]) return u;
    return parent[u] = uFind(parent[u]);
}

void merge (int u, int v) {
    u = uFind(u);
    v = uFind(v);
    if (u==v) return;
    parent[u] = v;
}


int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    
    n = 10;
    for (int i=1; i<=10; i++) {
        parent[i] = i;
    } 

}
```
하지만 위 코드를 사용하면 트리가 선형적일 때 uFind 함수를 사용하는데 {O(n)}이 걸릴 수 있다.

선형 트리를 만들지 않기 위해 Union Rank 최적화를 사용해야한다. ran[i] : 대표가 i인 tree의 높이

```c++
#include<bits/stdc++.h>

using namespace std;

int n;
int parent[11];
int ran[11];

int uFind (int u) {
    if (u==parent[u]) return u;
    return uFind(parent[u]);
}

void merge (int u, int v) {
    u = uFind(u);
    v = uFind(v);
    if (u==v) return;
    if (ran[u]>ran[v]) swap(u,v);
    parent[u] = v;
    if (ran[u]==ran[v]) ran[v]++;
}


int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    
    n = 10;
    for (int i=1; i<=10; i++) {
        parent[i] = i;
        ran[i] = 1;
    } 

}

```
