## Problem
<https://www.acmicpc.net/problem/7173>

implement + math


## Comment
단순 구현 문제...


## Code

```c++
#include<bits/stdc++.h>

using namespace std;

int m,n;
string arr[205];
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};

double preferStudent(int x, int y) {
    double ret(0),cnt(0);
    for (int i=0; i<4; i++) {
        int nx = x + dx[i];
        int ny = y + dy[i];
        if (nx<0||nx>=m||ny<0||ny>=n) continue;
        cnt++;
        ret += abs(arr[nx][ny]-arr[x][y]);
    }
    
    return ret/cnt;
}
int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> m >> n;
    double ans(0);
    for (int i=0; i<m; i++)
        cin >> arr[i];
    for (int i=0; i<m; i++) {
        for (int j=0; j<n; j++) {
            ans += preferStudent(i,j);
        }
    }
    
     cout << fixed << setprecision(4) << ans << '\n';
    
    return 0;
}


```
