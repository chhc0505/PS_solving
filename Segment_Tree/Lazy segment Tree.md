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
void init(int node, int start, int end) {
    if (start == end) {
        tree[node] = a[start];
    } else {
        init(a, tree, node*2, start, (start+end)/2);
        init(a, tree, node*2+1, (start+end)/2+1, end);
        tree[node] = tree[node*2] + tree[node*2+1];
    }
}
void update_lazy(int node, int start, int end) {
    if (lazy[node] != 0) {
        tree[node] += (end-start+1)*lazy[node];
        if (start != end) {
            lazy[node*2] += lazy[node];
            lazy[node*2+1] += lazy[node];
        }
        lazy[node] = 0;
    }
}
void update_range(int node, int start, int end, int left, int right, long long diff) {
    update_lazy(tree, lazy, node, start, end);
    if (left > end || right < start) {
        return;
    }
    if (left <= start && end <= right) {
        tree[node] += (end-start+1)*diff;
        if (start != end) {
            lazy[node*2] += diff;
            lazy[node*2+1] += diff;
        }
        return;
    }
    update_range(tree, lazy, node*2, start, (start+end)/2, left, right, diff);
    update_range(tree, lazy, node*2+1, (start+end)/2+1, end, left, right, diff);
    tree[node] = tree[node*2] + tree[node*2+1];
}

long long query(int node, int start, int end, int left, int right) {
    update_lazy(tree, lazy, node, start, end);
    if (left > end || right < start) {
        return 0;
    }
    if (left <= start && end <= right) {
        return tree[node];
    }
    long long lsum = query(tree, lazy, node*2, start, (start+end)/2, left, right);
    long long rsum = query(tree, lazy, node*2+1, (start+end)/2+1, end, left, right);
    return lsum + rsum;
}
```

출처 : https://book.acmicpc.net/ds/segment-tree-lazy-propagation
