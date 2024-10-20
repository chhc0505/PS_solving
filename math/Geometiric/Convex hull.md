## 블록껍질
- 다각형의 임의의 두 꼭짓점을 연결하는 선분이 항상 다각형 내부에 존재하는 다각형을 볼록 다각형이라고 한다
  
  -> 2차원 평면에 N개의 점이 주어졌을 때, 이들 중 몇 개의 점을 골라 볼록 다각형을 만드는데, 나머지 모든 점을 내부에 포함하도록 할 수 있다. 이를 볼록 껍질 (CONVEX HULL) 이라 한다.
  
  -> 볼록 껍질은 모든 각이 볼록하다.
  
  -> 볼록 껍질을 이루는 연속된 점 a,b,c를 골랐을 때 a,b,c가 반시계 방향을 이룬다.


## 구현 방법
1. y좌표가 가장 작은 점을 고른다. y좌표가 가장 작은 점이 여러 개라면, x좌표가 가장 작은 점을 고른다. 이 때 고른 점은 항상 블록 껍질을 이루는 점이다. 이는 직관적으로도 이해가 가능하다.
  
2. 주어진 정점을 기준점 기준으로 반시계 방향대로 정렬한다. 만약 ccw 값이 같다면 거리가 가까운 점이 앞에 오게 정렬한다.
 
3. stack에 정점 2개를 앞에서부터 넣는다.
  
4. 나머지 정점들을 차례대로 순회하는데, ccw(top-1, top, new_node)>0을 만족할 때까지 stack에서 pop해준다. (위를 만족하면 stack에 있는 원소는 new node와 모두 반시계를 이루는게 자명하다.)
  
5. 블록껍질에서 연속되는 점 a,b,c가 ccw(a,b,c)==0 을 만족하는 경우는 존재하지 않는다. 따라서 ccw 값이 0일 경우에도 pop해준다. (4번에서)

6. 4번 과정을 완료하면 stack에는 블록껍질의 정점들이 존재한다.


## Code
```c++
#include<bits/stdc++.h>
#define X first
#define Y second

using namespace std;

typedef long long ll;

int n;
vector<pair<ll,ll>> arr;
vector<pair<ll,ll>> stk;


ll ccw (pair<ll,ll>  vec1, pair<ll,ll>  vec2) {
    ll tmp = (vec1.X*vec2.Y - vec1.Y*vec2.X);
    return tmp;
}

bool cmp (pair<ll,ll> & a, pair<ll,ll> & b) {
    pair<ll,ll> stand = arr[0];
    pair<ll,ll> vec_a = {a.X-stand.X, a.Y-stand.Y};
    pair<ll,ll> vec_b = {b.X-stand.X, b.Y-stand.Y};

    ll tmp = ccw(vec_a, vec_b);
    if (tmp!=0) return tmp>0;
    else {
        return (vec_a.X*vec_a.X+vec_a.Y*vec_a.Y)<(vec_b.X*vec_b.X+vec_b.Y*vec_b.Y);
    }
}

int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n;

    for (int i=0; i<n; i++) {
        ll x,y; cin >> x >> y;
        arr.push_back({x,y});
    }

    sort(arr.begin(), arr.end(), [](pair<ll,ll> & a, pair<ll,ll> & b)->bool {if (a.Y!=b.Y) return a.Y<b.Y; return a.X<b.X;});
    sort(arr.begin()+1, arr.end(), cmp);

    stk.push_back(arr[0]);
    stk.push_back(arr[1]);

    for (int i=2; i<arr.size(); i++) {
        while (stk.size()>=2) {
            pair<ll,ll> top1 = stk[stk.size()-1];
            pair<ll,ll> top2 = stk[stk.size()-2];
            if (ccw({top1.X-top2.X, top1.Y-top2.Y}, {arr[i].X-top2.X, arr[i].Y-top2.Y})<=0)
                stk.pop_back();
            else break;
        }
        stk.push_back(arr[i]);
    }

    cout << stk.size() << '\n';

    return 0;
}
```
