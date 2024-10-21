## Lazy Segment Tree
다음과 같은 연산이 있다. 

1. 구간 l,r (l<=r)이 주어졌을 때, A[l]+A[l+1]...A[r-1]+A[r] 출력하기. (+가 아닌 다른 연산도 가능하다.)
2. i 번째 수부터 j 번째 수에 v를 더하기.

기본적인 세그먼트 트리를 사용하면 1번의 연산은 ${O(logn)}$로 해결이 가능하다.
하지만 2번의 경우, ${O(nlogn)}$이므로 비효율적이다. 
2번 연산을 빠르게 할 수 없을까 ?? 

SegmentTree의 경우 left idx부터 right idx까지 diff 만큼 더하는 함수를 다음과 같이 구현할 수 있다. 
```c++
void update_range(int node, int start, int end, int left, int right, long long diff) {
    if (left > end || right < start) {
        return;
    }
    if (start == end) {
        tree[node] += diff;
        return;
    }
    update_range(tree, node*2, start, (start+end)/2, left, right, diff);
    update_range(tree, node*2+1, (start+end)/2+1, end, left, right, diff);
    tree[node] = tree[node*2] + tree[node*2+1];
}

```
하지만 위 update_range 함수를 그대로 사용하면 앞서 설명했듯이 ${O(nlogn)}$으로 일반 배열선언보다 느리다.

하지만 세그먼트 트리에서 left<=start && end<=right 를 만족하는 node를 찾았다면, 세그먼트 트리의 리프 노드까지 재귀호출하지 않고 어떤 배열에 따로 저장할 수는 없을까 ? 

위 아이디어를 응요한 것이 바로 Lazy Segment Tree이다.

나중에 그 노드를 방문했을 때 전에 이런 update가 들어왔었어! 하고 알려주는 배열을 사용한다 - > lazy 배열

Lazy Segment Tree가 구현 가능하려면 노드에 대한 update에 대해서 리프노드가 아닌 상위 노드에서 lazy[node] 값이나 update 값의 결과를 리프노드를 방문하지 않고 구할 수 있어야한다 --> 핵심!!

리프노드를 하나하나 방문하지 않고 상위노드에서 미리 구현하고 리프노드의 lazy 배열에 추가해놓는다. 이 때문에 update_range를 ${O(nlogn)}$이 아닌 ${O(logn)}$에 구현이 가능하다. 

```c++
#include<bits/stdc++.h>


using namespace std;

typedef long long ll;

int n,m,k;
vector<ll> A,tree,lazy;

void init (int node, int start, int end) {
    if (start==end) {
        tree[node] = A[start];
        return;
    }

    init(2*node, start, (start+end)/2);
    init(2*node+1, (start+end)/2+1, end);
    tree[node] = tree[2*node]+tree[2*node+1];
}

void updateLazy (int node, int start, int end) {
    if (lazy[node]!=0) {
        tree[node] += (end-start+1)*lazy[node];
        if (start!=end) {
            lazy[2*node] += lazy[node];
            lazy[2*node+1] += lazy[node];
        }
        lazy[node] = 0;
    }
}

void updateRange (int node, int start, int end, int left, int right, ll diff) {
    updateLazy(node, start, end);
    if (start>right || end<left) {
        return;
    }

    if (left<=start && end<=right) {
        tree[node] += (end-start+1)*diff;
        if (start!=end) {
            lazy[2*node] += diff;
            lazy[2*node+1] += diff;
        }
        return;
    }

    updateRange(2*node, start, (start+end)/2, left, right, diff);
    updateRange(2*node+1, (start+end)/2+1, end, left, right, diff);
    tree[node] = tree[2*node]+tree[2*node+1];
}

ll query (int node, int start, int end, int left, int right) {
    updateLazy(node, start, end);
    if (start>right || end<left) {
        return 0;
    }
    if (left<=start && end<=right) {
        return tree[node];
    }

    ll lsum = query(2*node, start, (start+end)/2, left, right);
    ll rsum = query(2*node+1, (start+end)/2+1, end, left, right);
    return lsum+rsum;
}


int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n >> m >> k;
    tree.resize(4*n+5);
    lazy.resize(4*n+5);

    for (int i=0; i<n; i++) {
        ll num; cin >> num;
        A.push_back(num);
    }
    init(1,0,n-1);

    for (int i=0; i<m+k; i++) {
        int a; cin >> a;
        if (a==1) {
            ll b,c,d; cin >> b >> c >> d;
            updateRange(1,0,n-1,b-1,c-1,d);
        }else {
            ll b,c; cin >> b >> c;
            cout << query(1,0,n-1,b-1,c-1) << '\n';
        }
    }

    return 0;
}
```

출처 : https://book.acmicpc.net/ds/segment-tree-lazy-propagation
