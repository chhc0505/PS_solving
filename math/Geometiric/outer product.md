## 벡터의 외적


벡터의 외적은 3차원 이상에서 정의되는 개념으로, 결과값이 스칼라인 벡터의 내적과 다르게 결과 값이 벡터이다.

또한 a,b 두 벡터의 외적의 크기는 a와 b가 만드는 평행사변형의 넓이를 의미한다.

3차원에서의 외적 C 공식은 다음과 같다.

![image](https://github.com/user-attachments/assets/a84a9c11-9f00-4a38-bda4-033138e2b707)

![image](https://github.com/user-attachments/assets/43e5572b-b5a3-4188-8f31-1b4fece9ef1d)

또한 벡터의 방향은 오른손 법칙을 사용해서 구할 수 있다.

2차원에서의 외적은 정의되어있지 않지만, 3차원에서 z가 0이라고 생각하고, 문제를 해결한다.

대표적으로 외적을 활용해서 다각형의 크기를 구할 수 있다.

https://www.acmicpc.net/problem/2166


## CCW 알고리즘
CCW 알고리즘은 평면 상의 서로 다른 세 점이 주어졌을 때 (세 점은 한 평면을 의미, 무조건 성립)

벡터의 외적을 이용해서 세 점 사이의 관계를 파악하는 알고리즘이다. (시계, 반시계, 평행)

외적의 값이 양수이면 시계반대방향, 음수이면 시계방향, 0이면 평행의 관계이다. --> 오른손 법칙 생각!


CCW 알고리즘을 이용해서 두 선분의 교차여부를 판단할 수 있다.

![image](https://github.com/user-attachments/assets/5cb7f466-df45-48fa-9d23-eb33c8a8cbb0)

일반적으로 선분이 교차하는 경우 벡터 AB 와 벡터 AD를 외적한 값과 벡터 AB와 벡터 AC를 외적한 값을 곱한 결과가 음수이다. (한 쪽은 시계, 한쪽은 반시계)

![image](https://github.com/user-attachments/assets/2fcf99dc-e008-4870-bc62-eaa710d90cf8)

선분이 교차하지 않는 경우 외적값과 외적값을 곱한 결과가 양수이다.

![image](https://github.com/user-attachments/assets/2420e3a1-5507-4662-a8d9-6db111d547cb)

하지만 위와 같은 경우에, 선분이 교차하지 않음에도 외적과 외적을 곱한 값이 음수가 나올 수 있다. => 다른 선분을 기준으로하면 양수

따라서 각각의 선분을 기준으로 벡터를 구한 뒤, 외적과 외적을 곱해주어야한다.

![image](https://github.com/user-attachments/assets/08c9b0f4-6786-4e1f-84b6-7f4f13bb27ea)

위와 같으 경우는 외적을 구해서 곱하면 0이 나온다. 이 때 A<D && B>C 이면 교차, 아니면 교차가 아니다. (AB 와 CD는 오름차순이라고 가정한다.)


```c++
#include<bits/stdc++.h>
#define X first
#define Y second

using namespace std;

typedef long long ll;

int CCW (pair<ll,ll> v1, pair<ll,ll> v2) {
    ll s = v1.X*v2.Y- v2.X*v1.Y;
    if (s>0) return 1;
    if (s==0) return 0;
    if (s<0) return -1;
}

int intersect (pair<ll,ll> A, pair<ll,ll> B, pair<ll,ll> C, pair<ll,ll> D) {
    pair<ll,ll> AB = {B.X-A.X, B.Y-A.Y};
    pair<ll,ll> CD = {D.X-C.X, D.Y-C.Y};

    int ABC = CCW(AB, {C.X-A.X, C.Y-A.Y});
    int ABD = CCW(AB, {D.X-A.X, D.Y-A.Y});
    int CDA = CCW(CD, {A.X-C.X, A.Y-C.Y});
    int CDB = CCW(CD, {B.X-C.X, B.Y-C.Y});

    if (ABC*ABD==0 && CDA*CDB==0) {
        if (A>B) swap(A,B);
        if (C>D) swap(C,D);
        return ((A<=D && B>=C));
    }

    return (ABC*ABD<=0 && CDA*CDB<=0);
}

int main () {

    ios::sync_with_stdio(0); cin.tie(0);
    pair<ll,ll> A,B,C,D;
    cin >> A.X >> A.Y >> B.X >> B.Y;
    cin >> C.X >> C.Y >> D.X >> D.Y;
    
    int ans = intersect(A,B,C,D);
    cout << ans << '\n';



    return 0;
}
```
