## Query
크기가 N인 정수 배열 A가 있다. 다음의 쿼리를 최대 M번 수행한다고 가정해보자.

1. 특정 연산 ◎에 대하여 구간 l,r (l<=r)이 주어졌을 때  A[l] ◎ a[l+1] ◎ .... ◎ .. A[r-1] ◎ A[r] 구하기.

2. i 번째 수를 v로 바꾸기 (A[i] = v)

## 누적 합
- 1번 연산 : ${O(1)}$

- 2번 연산 : ${O(N)}$


## Segment Tree
세그먼트 트리를 사용하면 복잡도를 다음과 같이 줄일 수 있다.
- 1번 연산 : ${O(logN)}$

- 2번 연산 : ${O(logN)}$

### 구현
세그먼트 트리의 리프노드와 리프노드가 아닌 노드는 다음과 같은 의미를 가집니다.

- 리프 노드 : 배열의 수 그 자체

- 리프 노드가 아닌 노드 : 왼쪽 자식과 오른쪽 자식의 특정 연산 ◎ 값을 저장

![image](https://github.com/user-attachments/assets/237d5f55-edad-4f31-ab78-8d2937fcebac)

리프노드를 제외한 다른 모든 노드는 항상 2개의 자식을 가집니다. -> 세그먼트 트리는 Full binary tree (정 이진 트리)입니다.

만약 N이 2의 제곱인 경우에는 모든 노드가 항상 2개의 자식을 가지는 Perfect binary tree (포화 이진 트리) 입니다. 

리프 노드가 K개인 Full binary tree는 리프 노드가 아닌 노드가 K-1개 있습니다.

-> 트리의 전체 노드 개수가 T일 때 간선의 개수는 T-1, 리프 노드가 아닌 노드*2 = 간선의 개수 T-1 의 식을 이용해면 증명 가능하다. 

N이 2의 제곱꼴이 아닌 경우에 세그먼트 트리의 높이 H = [logN] 입니다. 

```c++
// a: 배열 A
// tree: 세그먼트 트리
// node: 노드 번호
// node에 저장되어 있는 합의 범위가 start - end
void init (int node, int start, int end) {
    if (start == end) {
        tree[node] = a[start];
    } else {
        init(a, tree, node*2, start, (start+end)/2);
        init(a, tree, node*2+1, (start+end)/2+1, end);
        tree[node] = tree[node*2] + tree[node*2+1];
    }
}
```
- node의 왼쪽 자식은 2*node, 오른쪽 자식은 2*node+1!
  
- segment tree에서 node를 root로 하고 [start, end] 범위를 초기화하는 함수

```c++
long long query (int node, int start, int end, int left, int right) {
    if (left > end || right < start) {
        return 0;
    }
    if (left <= start && end <= right) {
        return tree[node];
    }
    long long lsum = query(tree, node*2, start, (start+end)/2, left, right);
    long long rsum = query(tree, node*2+1, (start+end)/2+1, end, left, right);
    return lsum + rsum;
}
```
- 현재 [start,end]의 범위값을 가진 node를 방문했고, [left,right]에 해당하는 부분을 return하는 함수
  
범위 [start,end]에서 [left,right] 값을 구한다고 생각하자.

[start,end]라는 범위를 [start,(start+end)/2], [(start+end)/2+1, end] 로 쪼개고 [left,right]와 쪼갠 범위를 확인한다.

각 레벨에서 최대 4개의 노드만 방문한다. 만약 2개 이하의 노드를 방문했을 때 다음 레벨에서 4개 이하의 노드를 방문하는 것은 자명하다.

3개의 노드를 방문했을 때? 그 노드를 a,b,c 라고 하자. b는 절대 재귀호출을 하지 않으므로 다음 레벨에서 4개 이하의 노드를 방문한다.

4개의 노드를 방문했을 때 가장 왼쪽과 오른쪽에 있는 노드만 재귀호출을 함으로 다음 레벨에서 4개 이하의 노드를 방문한다.

따라서 시간복잡도가 ${O(logn)}$ 인게 자명하다.

```c++
void update (int node, int start, int end, int index, long long val) {
    if (index < start || index > end) {
        return;
    }
    if (start == end) {
        a[index] = val;
        tree[node] = val;
        return;
    }
    update(a, tree,node*2, start, (start+end)/2, index, val);
    update(a, tree,node*2+1, (start+end)/2+1, end, index, val);
    tree[node] = tree[node*2] + tree[node*2+1];
}
```
- 현재 [start,end]의 범위값을 가진 node를 방문했고 a[index]가 포함된 tree의 모든 부분을 update하는 함수
  
- update 함수 또한 log의 시간복잡도이다. 

```c++
int h = (int)ceil(log2(n));
int tree_size = (1 << (h+1));
```
트리의 높이 h와 tree의 size이다. 보통 tree의 size는 4*배열의 크기로 지정하는 경우도 많다.

출처 : https://book.acmicpc.net/ds/segment-tree
