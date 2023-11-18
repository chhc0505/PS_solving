## Problem
<https://www.acmicpc.net/problem/9252>
DP + String
    
## Coment
* LCS의 길이를 계산하는 DP 문제.
    DP의 index에 string의 index를 매치하고 DP를 쌓아야한다.
    __dp[i][j] : 문자열 s1[0,i+1], s2[0,j+1] 에서의 LCS 최대 길이__
    역 추적을 위해 dp 이전의 정보를 node에 저장

## Code

```c++
#include<bits/stdc++.h>

using namespace std;

string s1, s2;

struct node {
    int LCS,x,y;
};

node dp[1005][1005]; // 

int main () {
    
    ios::sync_with_stdio(0); cin.tie(0);
    cin >> s1 >> s2;
    node node1 = {0,0,0};
    fill(&dp[0][0],&dp[1004][1005],node1);
    
    for (int i=1; i<=s1.size(); i++) {
        for (int j=1; j<=s2.size(); j++) {
            if (s1[i-1] == s2[j-1]) {
                dp[i][j].LCS = dp[i-1][j-1].LCS + 1;
                dp[i][j].x = i-1; dp[i][j].y = j-1;
            } else if (dp[i-1][j].LCS > dp[i][j-1].LCS) {
                dp[i][j].LCS = dp[i-1][j].LCS;
                dp[i][j].x = i-1; dp[i][j].y = j;
            } else {
                dp[i][j].LCS = dp[i][j-1].LCS;
                dp[i][j].x = i; dp[i][j].y = j-1;
            }
        }
    }
    
    deque<char> ans;
    
    node prev_node = dp[s1.size()][s2.size()];
    if (s1[s1.size()-1] == s2[s2.size()-1]) ans.push_front(s1[s1.size()-1]);
    while (!(prev_node.x==0 && prev_node.y==0)) {
        if (s1[prev_node.x-1] == s2[prev_node.y-1]) ans.push_front(s1[prev_node.x-1]);
        prev_node = dp[prev_node.x][prev_node.y];
    }
    
    cout << dp[s1.size()][s2.size()].LCS << '\n';
    for (auto & c : ans)
        cout << c;
    cout << '\n';
        
    
    return 0;
}
```
