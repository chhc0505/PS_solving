## Problem
<https://www.acmicpc.net/problem/30168>

sccc 내전3회-E

__math__

## Comment
* 배열의 i,j를 1) 무조건 하얀색인 경우, 2) 무조건 검은색인 경우, 3) 하얀색 또는 검은색 모두 상관없는 경우를 나누어서 생각해야한다.
  1) 무조건 하얀색인 경우 : r[i] = j, c[j] = i 인 경우
  2) 무조건 검은색인 경우 : r[i] <= j, c[j] <= i 인 경우
  3) 상관없는 경우 : 나머지

* 주어진 r과 c의 배열로 구현이 불가능한 경우는 i,j 번째 색이 1)과 2)상황이 동시에 일어났을 때이다.

## Code
```c++
#include<bits/stdc++.h>

using namespace std;

int h,w;
vector<int> r,c;
int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> h >> w;
    for (int i=0; i<h; i++) {
        int num; cin >> num;
        r.push_back(num);
    }
    
    for (int i=0; i<w; i++) {
        int num; cin >> num;
        c.push_back(num);
    }
    long long ans = 1;

    for (int i=0; i<h; i++) {
        for (int j=0; j<w; j++) {
            if ((r[i]==j&&c[j]>i)||(r[i]>j&&c[j]==i)) {
                cout << 0 << '\n';
                return 0;
            } else if ((r[i]==j||c[j]>i)||(r[i]>j||c[j]==i)) 
                continue;
            else {
                ans*=2;
                ans = ans % (long long)(1e9+7);
            } 
        }
    }
    cout << ans << '\n';
    
    return 0;
}
```
