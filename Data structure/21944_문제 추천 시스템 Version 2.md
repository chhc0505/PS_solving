## Problem
<https://www.acmicpc.net/problem/21944>

__Data-structure - set, hash__

## Comment
* 전체 문제를 저장하는 set, 알고리즘 G별로 저장하는 set을 따로 관리.

  solve 명령어를 위한 p->{l,g} hash 관리


## Code
```c++
#include<bits/stdc++.h>

using namespace std;

int n,m;
set<pair<int,int> > problem_list;
set<pair<int,int> > list_by_level [101];
unordered_map<int,pair<int, int> > problem2data;
int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> n;
    for (int i=0; i<n; i++) {
        int p,l,g; cin >> p >> l >> g;
        problem_list.insert({l,p});
        list_by_level[g].insert({l,p});
        problem2data[p] = {l,g};
    }
    cin >> m;
    for (int i=0; i<m; i++) {
        string command; cin >> command;
        if (command == "recommend") {
            int g,x; cin >> g >> x;
            if (x==1) cout << (*prev(list_by_level[g].end())).second << '\n';
            else cout << (*(list_by_level[g].begin())).second << '\n';
        } else if (command == "recommend2") {
            int x; cin >> x;
            if (x==1) cout << (*prev(problem_list.end())).second << '\n';
            else cout << (*problem_list.begin()).second << '\n';
        } else if (command == "recommend3") {
            int x,l; cin >> x >> l;
            if (x==1) {
                auto iter = problem_list.lower_bound({l,1});
                if (iter == problem_list.end()) cout << -1 << '\n';
                else cout << (*iter).second << '\n';
            } else {
                auto iter = problem_list.lower_bound({l,1});
                if (iter == problem_list.begin()) cout << -1 << '\n';
                else cout << (*(prev(iter))).second << '\n';
            }
        } else if (command == "add") {
            int p,l,g; cin >> p >> l >> g;
            problem_list.insert({l,p});
            list_by_level[g].insert({l,p});
            problem2data[p] = {l,g};
        } else if (command == "solved") {
            int p,l,g; cin >> p;
            l = problem2data[p].first; g = problem2data[p].second;
            problem_list.erase({l,p});
            list_by_level[g].erase({l,p});
        }
    }
     
    return 0;
}
```
