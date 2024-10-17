## Easy DP
**1. 계단 오르기 (S3)**
 
  ${dp[i][0] =  \max(dp[i-2][0],\ dp[i-2][1])+scores[i] }$

  ${dp[i][1] = dp[i-1][0]+scores[i]}$


  
