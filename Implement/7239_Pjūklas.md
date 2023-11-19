## Problem
<https://www.acmicpc.net/problem/7239>

sccc 내전3회 - B

Ad hoc + math


## Comment
* 정렬한 뒤 양 사이드 index를 차례대로 순회하면 주어진 해를 구성할 수 있다

* 홀수와 짝수일 때의 예외처리 해주기 !


## Code
```c++
#include<bits/stdc++.h>

using namespace std;

int n;
vector<int> A;

int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n;
    for (int i=0; i<n; i++) {
        int num; cin >> num;
        A.push_back(num);
    }
    sort(A.begin(),A.end());
    
    int p1(0),p2(n-1);
    if (n==1) {
        cout << A[0] << '\n';
        return 0;
    }
    
    while (true) {
        cout << A[p1] << ' ';
        cout << A[p2] << ' ';
        p1++; p2--;
        if (p1>=p2) break;
    }
    if (n%2==1) cout << A[p1] << '\n';
    else cout << '\n';
    
    return 0;
}

```
