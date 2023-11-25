## Problem
<https://www.acmicpc.net/problem/21939>

__Data structrue - set, unordered_map__

## Comment
* set을 사용하면 쉽게 풀 수 있지만 erase 때문에 잠깐 애먹었던 문제.
  
  erase에는 문제의 번호만 주어지기 때문에 문제의 번호 - 난이도로 이어지는 hash table을 사용해야 해결할 수 있다.

## Code
```c++
#include<bits/stdc++.h>

using namespace std;

int n,m;

set<pair<int,int> > problems;
unordered_map<int,int> number2level;
int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n;
    
    for (int i=0; i<n; i++) {
        int p,l; cin >> p >> l;
        problems.insert({l,p});
        number2level[p] = l;
    }
    
    cin >> m;
    
    for (int i=0; i<m; i++) {
        string command; cin >> command;
        if (command == "recommend") {
            int num; cin >> num;
            if (num==1) cout << (*prev(problems.end())).second << '\n';
            else if (num==-1) cout << (*problems.begin()).second << '\n';
        } else if (command == "add") {
            int p,l; cin >> p >> l;
            problems.insert({l,p});
            number2level[p] = l;
        } else {
            int p; cin >> p;
            problems.erase({number2level[p],p});
        }
    }
    
    return 0;
}
```
