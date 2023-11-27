## Problem

숭실대학교 2학년 알고리즘 과제 - #HW5

__Greedy__

## Comment
* 직사각형 두부의 크기가 한 변이 sx, 다른 한 변이 sy라고 하자

  이 때 두부가 최소의 개수가 되게 직사각형 두부를 자르려면 반드시 __직사각형의 작은 변을 한 변으로 하는 정사각형__ 으로 잘라야한다.

  만약 직사각형의 작은 변을 한 변으로 하는 정사각형을 자르지 않았을 때 직사각형 sx, sy 두부를 최소의 개수로 자를 수 있다 가정해보자. (귀류법)

  sx, sy 직사각형의 작은 변보다 큰 길이로는 직사각형에서 정사각형씩 자를 수 없다.

  만약 sx, sy 직사각형의 작은 변보다 작은 길이로 자르면 최대로 잘랐을 때 남은 두부의 크기가 작은 변을 길이로하는 정사각형을 이용해 최대로 잘랐을 때보다 클 수 밖에 없다.  

## Code
```c++
#include <bits/stdc++.h>

using namespace std;

long long total_cnt(0),total_price(0);

void find_minimun_topu(long long sx, long long sy){
	long long s_length,b_length,plus_cnt;
	s_length = (sx>sy?sy:sx);
	b_length = (sx>sy?sx:sy);
	if (b_length%s_length==0) {
		total_cnt += b_length/s_length;
		total_price += b_length;
		return;
	}
	plus_cnt = b_length/s_length;
	total_cnt += plus_cnt; total_price += s_length*plus_cnt;
	find_minimun_topu(s_length, b_length-s_length*plus_cnt);
}

int main () {
  long long int x,y;
  cin >> x >> y;
  find_minimun_topu(x,y);
  cout << total_cnt << ' ' << total_price << '\n';
}
```
