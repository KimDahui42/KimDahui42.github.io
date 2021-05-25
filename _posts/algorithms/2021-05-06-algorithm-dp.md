---
layout: post
title: "DP"
date: 2021-05-06
excerpt: "동아리 발표자료"
tags: [study,algorithms,dp]
category: [Algorithms] 
comments: false
---
# DP
## 목차
* [동적 계획법](#동적-계획법이란-복잡한 문제를-간단한-여러-개의-문제로-나누어-푸는-방법을-의미합니다)
* [시간 복잡도](#dp문제는-식에-따라-시간복잡도가-달라지지만-단순하게-해당-식의-시간을-분석하는-방법은-다음과-같습니다)
* [성립조건](#dp가-성립하기-위한-조건)
* [top-down, bottom-up](#dp는-top-down-방식과-bottom-up-방식으로-구현합니다)
* [2xn 타일링](#11726번-2xn-타일링)
* [타일 채우기 3](#14852번 타일 채우기 3)
* [참고 문제](#참고-문제)


<br>

### 동적 계획법이란 복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법을 의미합니다
 이는 큰 의미에서 분할 정복과 같은 접근 방식이지만 둘은 문제를 나누는 방식에 차이가 있는데요, dp의 부분 문제는 두 개 이상의 작은 문제를 푸는데 사용될 수 있기 때문에 문제를 여러번 계산하는 대신 한 번만 계산하고, 결과를 dp배열에 저장하고 재활용하여 전체 계산 속도를 향상 시킬 수 있습니다. 이러한 특징을 메모이제이션이라고 칭합니다.
dp는 이 메모이제이션을 통해 부분 문제의 중복계산을 방지할 수 있습니다.
DP는 모든 경로를 확인하기 때문에 그당시의 최선의 해를 구하는 그리디 알고리즘에 비해 시간이 걸릴 수 있다는 단점이 있지만 항상 최적의 해를 구할 수 있습니다.
이러한 특징으로 dp는 경우의 수를 세거나 확률을 계산할 때도 흔히 사용됩니다.
<br>
### dp문제는 식에 따라 시간복잡도가 달라지지만 단순하게 해당 식의 시간을 분석하는 방법은 다음과 같습니다

`(존재하는 부분 문제의 수)X(한 부분 문제를 풀 때 필요한 반복문의 작성횟수)`

 이미 계산한 답을 사용하는 것은 상수시간이 사용되기 때문에 전체 수행 시간에 영향을 미치지 않습니다. 
<br>
### dp가 성립하기 위한 조건
* 작은 문제들이 반복된다
* 같은 문제는 구할 때마다 값이 같다

 경우의 수를 세는 문제같은 경우에 답이 입력의 크기에 대해 지수적으로 증가할 수 있어 쉽게 오버플로우가 발생할 수 있습니다. 때문에 오버플로우의 대처도 함께 고민하게됩니다.
<br>
### dp는 top-down 방식과 bottom-up 방식으로 구현합니다
#### top-down
 dp에 저장된 값이 없다면 재귀로 이전 항의 값을 호출 계산하고 이미 저장된 값이 있다면 저장된 값을 리턴하도록 합니다. 
```
/*예시로 든 코드는 피보나치 수열의 구현으로 피보나치 수열은 모든 항은 바로 앞 두 항의 합인 수열을 의미한다.
코드 작성의 편의상 0번째 항은 0, 첫번째 항은 1을 저장했다.*/

int dp[1000] = { 0, };
dp[0]= 0; dp[1] = 1;
int fibo_recur(int n) {
	if (n < 2)
		return dp[n];
	else if (dp[n] != 0)
		return dp[n];
	return dp[n] = (fibo_recur(n-1) + fibo_recur(n - 2));
}
```
#### bottom-up
 큰 문제를 작은 문제로 나누고 작은 문제에서 큰 문제로 답을 구해나가는 방식으로 풀이
```
int dp[1000] = { 0, };
dp[0]= 0; dp[1] = 1;
int fibo_repet(int n) {
	for (int i = 0; i <= n; i++) 
		dp[n] = dp[n - 1] + dp[n - 2];
	return dp[n];
}
```
<br>
### <a href="https://www.acmicpc.net/problem/11726">11726번 2xn 타일링</a>
<figure>
	<a href="/assets/etc/algorithms/2n타일링.JPG"><img src="/assets/etc/algorithms/2n타일링.JPG"></a>
</figure>
<br>
 문제를 보면 세로 2, 가로 n의 직사각형을 1x2, 2x1 타일로 채우는데요 <br>
 n이 1증가할 때 생기는 경우는 1가지, n이 2 증가할 때 생기는 경우는 2가지인 것을 확인할 수 있습니다. n이 3증가할 때는 1의 경우에 2의 경우를 더한만큼의 경우가 생성됩니다.<br> 
 이 규칙이 이후로도 동일하게 적용되어 n번째 항의 값은 앞의 두 항의 값을 더하는 것으로 구할 수 있게됩니다. 점화식으로 표현하면 dp[n]=(dp[n-1]+dp[n-2])이 되고, 문제의 조건에 맞게 두 항의 합을 10007로 나눈 나머지를 저장합니다.<br><br>
 코드로는 n을 입력받아 dp[n]에 이미 저장된 값이 있다면 값을 리턴, 아니라면 계산을 위한 값을 재귀적으로 호출하는 것으로 구현할 수 있습니다.
 ```
 #include<iostream>
using namespace std;
int dp[1001] = { 0, };
int dynamic(int n);
int main() {
	int n,ans;
	cin >> n;
	ans=dynamic(n);
	cout << ans << endl;
}
int dynamic(int n) {
	dp[1] = 1;
	dp[2] = 2;
	if (dp[n] != 0)
		return dp[n];
	return dp[n] = (dynamic(n-1) + dynamic(n - 2))%10007;
}
```
<br>
### <a href="https://www.acmicpc.net/problem/14852">14852번 타일 채우기 3</a>
<figure>
	<a href="/assets/etc/algorithms/타일 채우기3.JPG"><img src="/assets/etc/algorithms/타일 채우기3.JPG"></a>
</figure>
<br>
 2xn타일링 문제와 유사한 문제입니다. 먼저 규칙을 찾겠습니다. n이 1증가할 때 찾을 수 있는 경우의 수는 2가지, n이 2증가할 때 찾을 수 있는 경우의 수는 세가지입니다. 하지만 n이 3 증가할때부터는 새로운 2가지 경우의 수가 추가되기때문에 그대로 작성하게 되면 문제가 발생합니다.<br>
 시간초과와 범위초과의 문제를 해결하기 위해 이차원 dp와 long long int 형을 사용하게됩니다.<br>
 이차원 dp의 경우 dp[x][0] 전체 경우의 수를, dp[x][1]에 새로 추가되는 경우의 수를 저장하게 됩니다. 새로 추가되는 경우의 수는 위아래를 뒤집은 것과 같기 때문에 편의상 하나씩만 계산하고 추후 2를 곱해주는 방식을 사용했습니다. <br>
dp[0][0]에는 0, dp[1][0]에는 2, dp[2][0]에는 7을 저장하고 dp[3]부터 새롭게 2가지씩의 경우를 추가하게 되기때문에 dp[2][1]에 1을 저장해줍니다. 
dp[i][1]은 2씩 고유의 값을 더해주기 위해 세번째 앞의 총 경우의 수 값과 바로 앞의 고유로 생성되는 경우의 수를 더해준 값을 기록합니다. 이미 dp[2]까지의 공간에 값을 저장했기때문에 3부터 구하고자하는 값 n까지 for문을 돌려 값을 구합니다. n번만큼계산하기 때문에 시간복잡도 역시 O(n)입니다.<br>
`dp[x][0]`은 `2*dp[x-1][0]`과 `3*dp[x-2][0]`을 더한 값에 `dp[x][1]*2`를 더해준 값을 기록합니다. 모든 값은 조건에 맞게 십억칠로 나눈 나머지 값을 기록해야합니다.<br>
```
#include<iostream>
#define MAX 1000000007;
using namespace std;
long long int dp[1000001][2] = { 0, };
long long int dynamic(int n);
int main() {
	int n;
	dp[0][0] = 0; dp[1][0] = 2; dp[2][0] = 7; dp[2][1] = 1;
	cin >> n;
	cout << dynamic(n) << endl;
	return 0;
}
long long int dynamic(int n) {
	for (int i = 3; i <= n; i++) {
		dp[i][1] = (dp[i - 1][1] + dp[i - 3][0]) % MAX;
		dp[i][0] = (3 * dp[i - 2][0] + 2 * dp[i - 1][0] + 2 * dp[i][1])%MAX;
	}
	return dp[n][0];
}
```
<br>
### 참고 문제
* <a href="https://www.acmicpc.net/problem/11053">11053번 가장 긴 증가하는 부분수열</a>
* <a href="https://www.acmicpc.net/problem/2133">2133번 타일 채우기</a>
* <a href="https://www.acmicpc.net/problem/12865">12865번 평범한 배낭</a>
* <a href="https://www.acmicpc.net/problem/10422">10422번 괄호</a>




 
 
