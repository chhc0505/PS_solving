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

string s;
long long arr[26]; // a~z 0~25
long long ans(0);
bool flag = false;
int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> s;
    for (int i=0; i<s.size(); i++)
        arr[s[i]-'a']+=1;
    
    for (int i=0; i<26; i++) {
        for (int j=i+1; j<26; j++) {
            ans += (arr[i]*arr[j]);
        }
    }
    for (int i=0; i<26; i++) {
        if (arr[i]>=2) {
            flag = true;
            break;
        }
    }
    if (flag) cout << ans+1 << '\n';
    else cout << ans << '\n';
    
    return 0;
}
```
