## Problem
<https://www.acmicpc.net/problem/7257>

Data structure - Hash

sccc 내전3회 - D

## Comment
* 각 잔디의 길이를 모두 관리하면 O(N*M)으로 주어진 시간 내에 해걸할 수 없다.
  
  따라서 지금까지 자른 잔디의 총 길이와 자르고 남은 잔디의 총 합을 따로 관리해줘야하고

  __i일이 지나고 남아있는 잔디는 deque나 stack같은 자료구조__ 로 관리해야한다. -> 시간복잡도 O(n+m)


## Code
```c++
#include<bits/stdc++.h>

using namespace std;

int n,m;
long long sum_g(0),sum_k(0);
deque<int> A;
deque<int> B;
int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n;
    for (int i=0; i<n; i++) {
        int num; cin >> num;
        sum_g += num;
        A.push_back(num);
    }
    cin >> m;
    for (int i=0; i<m; i++) {
        int num; cin >> num;
        B.push_back(num);
    }
    //cout << sum_g << '\n';
    sort(A.begin(),A.end());
    for (int day=0; day<m; day++) {
        sum_g += A.size();
        sum_k += B[day];
        while (!A.empty() && (A[0]+day+1<=sum_k)) {
            sum_g -= (A[0]+day+1-sum_k+B[day]);
            A.pop_front();
        }
        sum_g -= B[day]*A.size();
        cout << sum_g << '\n';
    }
    
    return 0;
}
```
