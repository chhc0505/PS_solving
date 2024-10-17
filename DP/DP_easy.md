## Easy DP
${\textsf{\color{lightblue}2579. 계단 오르기 (S3)}}$

- dp[i][j] : j-> i-1번째 계단을 밟았는가? i 번째 계단을 밟았을 때의 최대 점수

- ${dp[i][0] =  \max(dp[i-2][0],\ dp[i-2][1])+scores[i] }$

  ${dp[i][1] = dp[i-1][0]+scores[i]}$

${\textsf{\color{lightblue}9465. 스티커 (S1)}}$

- dp[i][j] : 0~i개의 스티커 중 (j,i) 스티커를 떼어냈을 때 스티커의 점수의 최댓값

* ${dp[i][0]\ = \max(dp[i-1][1],\ dp[i-2][1])+scores[i]}$

  ${dp[i][1]\ = \max(dp[i-1][0], \ dp[i-2][0])+scores[i]}$


${\textsf{\color{gold}2293. 동전 1 (G4)}}$

- filling DP

- dp[i] : i원을 만드는 경우의 수

- coin[0], coin[1], ... coin[n-1] 차례로 채우기.

${\textsf{\color{lightblue}9461. 파도반 수열 (S3)}}$
  
- ${dp[i]\ =\ dp[i-1]+dp[i-5]}$


${\textsf{\color{gold}2294. 동전 2 (G5)}}$

-  ${dp[i] += \sum_{j=0,\ i-coin[j]\geq0}^{n-1} dp[i-coin[j]]}$

${\textsf{\color{gold}2133. 타일 채우기 (G4)}}$

- ${dp[i] = dp[i-2]+ \sum_{j=0,\ j=j+2}^{i} dp[i-j]}$

${\textsf{\color{gold}1520. 내리막길 (G3)}}$

- dfs DP

- dp[i][j] : 위치 (i,j)에서 (n-1,m-1)에 도달할 수 있는 경우의 수

${\textsf{\color{gold}1520. 점프 (S1)}}$

- dfs DP

- dp[i][j] : 위치 (i,j)에서 (n-1,n-1)에 도달할수 있는 경우의 수

${\textsf{\color{gold}1520. 합분해 (G5)}}$

- dp[i][j] : 0부터 N까지의 정수 j개를 더해서 그 합이 i가 되는 경우의 수

- ${dp[i][j]\ = \sum_{k=1}^{i} dp[i-k][j-1]


