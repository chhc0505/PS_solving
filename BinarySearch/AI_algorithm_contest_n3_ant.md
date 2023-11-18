## Problem
숭실대학교 AI융합학부 Code Challenge 3번 문제

Parametric Search + Greedy

## Coment
* Point1 : 최소시간을 기준으로 최솟값을 찾는 문제를 이분탐색 결정문제로 생각할 수 있는가 - 만약 데이터가 정렬되어 있고 특정 조건에 따라 이분적으로 나뉜다면 이분탐색 생각 !

* Point2 : 특정 시간 내에 개미가 꿀을 먹을 수 있는지 check 하는 함수를 구현할 수 있는가 - 개미가 x초 이내에 꿀을 먹을 수 있다 -> 모든 개미 위치 p[i], [p[i]-x, p[i]+x] 위치 내에 적어도 꿀 하나 존재로 해석 !

* Point3 : 꿀을 그리디하게 배정할 수 있는가 - 배열 p와s를 오름차순으로 정렬하였을 때 가장 최적으로 꿀을 배정하려면 개미 범위 [p[i]-x, p[i]+x] 에서 가장 앞에 있는 꿀을 사용해야한다 !

## Code
```c++
#include<bits/stdc++.h>


using namespace std;


int n,m,k;
vector<long long> p,s;

bool canInstallHoney (long long x) {
	bool flag = true;
	int ant_p = p.size()-1,honey_p = s.size()-1,honey_cnt(0);
	
	while (ant_p>=0 && honey_p>=0) {
		bool honey_use = false;
		while (honey_p-1>=0 && (p[ant_p]-x<= s[honey_p-1]) && (s[honey_p-1] <= p[ant_p]+x)) {
			honey_p--; 
		}
		while (ant_p>=0 && (p[ant_p]-x<= s[honey_p]) && (s[honey_p] <= p[ant_p]+x)) {
			ant_p--;
			honey_use = true;
		}
		if (honey_use) honey_cnt++;
		honey_p--;
		if (honey_cnt>k) {
			flag = false;
			break;
		}
	}
	
	if (ant_p>=0 && honey_p<0) {
		flag = false;
	}
	return flag;
}

int myBinarySearch () {
	long long st(0),ed(2e9);
	while (st+1<ed) {
		long long mid = (st+ed)/2;
		bool LorR = canInstallHoney(mid);
		if (LorR) ed = mid;
		else st=mid;
	}
	if (canInstallHoney((st+ed)/2)) return (st+ed)/2;
	else return (st+ed)/2+1;
}
int main () {
	
	ios::sync_with_stdio(0); cin.tie(0);
	cin >> n >> m >> k;
	for (int i=0; i<n; i++) {
		int num; cin >> num;
		p.push_back(num);
	}
	for (int i=0; i<m; i++) {
		int num; cin >> num;
		s.push_back(num);
	}
	sort(p.begin(),p.end());
	sort(s.begin(),s.end());

	cout << myBinarySearch() << '\n';
	

	return 0;

}
```
