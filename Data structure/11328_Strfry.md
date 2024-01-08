## Problem
<https://www.acmicpc.net/problem/11328>

Data structue - array

## Comment
* std namespace, algorithm 헤더에 있는 fill 함수는 fill(처음 주소, 마지막 주소+1, 초기화 값) 이다 !

* global한 전역배열의 초기화는 runtime 전에 초기화가 되기 때문에 시간이 걸리지 않는다.

  하지만 함수 내 선언된 지역배열의 초기화는 O(n)의 시간복잡도가 걸린다. (compiler가 memset이나 fill로 optimize하는 경우도 있다.)

  memset과 fill함수는 일반적으로 O(n)이지만 실제로 더 적은 복잡도를 가진다. (현대 프로세서 instruction이 발전했기 때문! )

## Code
```c++
#include<bits/stdc++.h>

using namespace std;

int n,arr[26];
int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n;
    
    for (int i=0; i<n; i++) {
        fill(&arr[0],&arr[26],0);
        bool flag = true;
        string s1,s2; cin >> s1 >> s2;
        for (auto c : s1)
            arr[c-'a']++;
        for (auto c : s2)
            arr[c-'a']--;
        for (int i=0; i<26; i++) {
            if (arr[i]!=0) flag = false;
        }
        if (flag) cout << "Possible" << '\n';
        else cout << "Impossible" << '\n';
    }
    
    return 0;
}
```
