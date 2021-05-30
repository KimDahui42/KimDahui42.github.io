---
layout: post
title: "트리DP"
date: 2021-05-30
excerpt: "동아리 발표자료"
tags: [study,algorithms,dp,treeDP]
category: [Algorithms] 
comments: false
---
# Tree DP
## 목차
* [Tree Dp](#트리-dp는-dp의-풀이법을-트리에-적용한-방식)
* [트리의 독립집합](#2213번-트리의-독립집합)
* [사회망 서비스](#2533번-사회망-서비스)
* [참고 문제](#참고-문제)

<br>

### 트리 dp는 dp의 풀이법을 트리에 적용한 방식
`서브 트리에서 구한 해를 이용해 전체 트리의 해를 구합니다` 트리를 사용하기 때문에 탐색 순서를 지정해주어야하며 주로 dfs를 활용합니다. dfs를 통해 풀이한 답을 dp배열에 저장합니다.

<br>

### <a href="https://acmipc.net/problem/2213">2213번 트리의 독립집합</a>

<figure>
	<a href="/assets/etc/algorithms/트리의 독립집합.JPG"><img src="/assets/etc/algorithms/트리의 독립집합.JPG"></a>
</figure>

<br> 문제에서 정점의 부분 집합 s에 속한 모든 정점쌍이 서로 인접하지 않으면 s를  독립 집합이라 지칭한다 제시되어있습니다. 그리고 트리의 각 정점에는 가중치가 주어지고, 가중치의 합을 이용하여 최대 독립 집합을 구하게 됩니다. 
<br> 이를 통해 독립집합에 속해있는 노드끼리 엣지가 없게 하기 위해서는 어떤 노드를 독립집합에 넣는다면 그 자식 노드들은 독립 집합에 포함하지 않아야합니다.

> **정리하면 다음과 같습니다.**
>> * `dp[x][0]`
>> - 정점 x가 독립 집합에 포함되지 않은 경우의 최댓값을 뜻한다. 
>> - 자식 노드의 독립 집합을 포함할 수도, 하지않을 수도 있다.
>> 두 경우 중 더 값이 큰 경우를 더해 저장한다.
>> * `dp[x][1]`
>> - 정점 x가 독립 집합에 포함된 경우.
>> - 자식 노드를 포함하지 않은 경우를 더해 저장한다.

아래 코드에는 두 개의 for문이 존재하는데, 첫번째 for 문은 연결된 노드들에 대해 dfs를 수행합니다. 현재 노드를 포함하지 않은 경우는 값이 0, 포함된 경우 해당하는 가중치가 dp에 저장되며 두번째 for 문을 통해 정리했던 내용을 적용합니다.

```
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int n, w[10001], dp[10001][2];
vector<int>v[10001], result;
void dfs(int now, int parent);
void trace(int now, int isInclude, int parent);
int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> w[i];
	}
	for (int i = 0, a, b; i < n - 1; i++) {
		cin >> a >> b;
		v[a].push_back(b);
		v[b].push_back(a);
	}
	dfs(1, 0);
	cout << max(dp[1][0], dp[1][1]) << '\n';
	if (dp[1][0] < dp[1][1]) {
		trace(1, 1, 0);
	}
	else
		trace(1, 0, 0);
	sort(result.begin(), result.end());
	for (int i = 0; i < (int)result.size(); i++)
		cout << result[i] << " ";
	return 0;
}
void dfs(int now, int parent) {
	for (int i = 0; i < v[now].size(); i++) {
		if (v[now][i] == parent) continue;
		else dfs(v[now][i], now);
	}
	dp[now][0] = 0;
	dp[now][1] = w[now];
	for (int i = 0; i < v[now].size(); i++) {
		if (v[now][i] == parent) continue;
		dp[now][0] += max(dp[v[now][i]][0], dp[v[now][i]][1]);
		dp[now][1] += dp[v[now][i]][0];
	}
}
void trace(int now, int isInclude, int parent) {
	if (isInclude == 1) {
		result.push_back(now);
		for (int i = 0; i < v[now].size(); i++) {
			if (v[now][i] == parent)continue;
			trace(v[now][i], 0, now);
		}
	}
	else {
		for (int i = 0; i < v[now].size(); i++) {
			if (v[now][i] == parent)continue;
			if (dp[v[now][i]][0] < dp[v[now][i]][1])
				trace(v[now][i], 1, now);
			else trace(v[now][i], 0, now);
		}
	}
}

```

<br>

### <a href="https://acmipc.net/problem/2533">2533번 사회망 서비스</a>

<figure>
	<a href="/assets/etc/algorithms/사회망 서비스.JPG"><img src="/assets/etc/algorithms/사회망 서비스.JPG"></a>
</figure>

<br> 문제는 모든 사람이 새로운 아이디어를 수용하기 위해 필요한 최소 얼리어답터의 수를 구해야합니다. 얼리어답터만 아이디어를 퍼뜨릴수 있기 때문에 최소 얼리어답터를 구하기 위해서는 얼리어답터는 떨어져있어야 하며 현재 노드가 얼리어답터가 아니라면 자식들은 얼리어답터일 수도, 아닐 수도 있으나 적어도 한 명은 얼리어답터야 아이디어를 전파할 수 있습니다.
<br> 이러한 특성으로 앞선 트리의 독립 집합 문제와 유사한 문제가 됩니다.

> **정리하면 다음과 같습니다.**
>> * `dp[x][0]`
>> - 정점 x가 얼리어답터에 포함되지 않은 경우의 최솟값을 뜻한다. 
>> - 자식 노드를 포함한 경우를 더해 저장한다.<br>
>> * `dp[x][1]`
>> - 정점 x가 독립 집합에 포함된 경우.
>> - 자식 노드의 독립 집합을 포함할 수도, 하지않을 수도 있다.
>> - 두 경우 중 더 값이 작은 경우를 더해 저장한다.

메인 함수에서 dp는 -1로 초기화 되어있습니다. 방문사실을 기록하고 독립집합 문제와 달리 가중치가 없으니 얼리어답터여부가 결과의 초기값이 됩니다. 연결된 노드들에 다음 노드를 방문한 적 있다는 것은 부모 노드임을 의미해 무시하고 정리한 내용을 적용하며 최솟값을 계산하고 얼리어답터여부 모두를 체크 하기 위해 방문사실을 지워줍니다

```
#include<iostream>
#include<algorithm>
#include<vector>
#include<cstring>

using namespace std;

int n, dp[1000001][2];
bool chk[1000001];
vector<int>v[1000001];

int dfs(int now,int isInclude);

int main(){
    cin>>n;
    for(int i=0,a,b;i<n-1;i++){
        cin>>a>>b;
        v[a].push_back(b);
        v[b].push_back(a);
    }
    memset(dp,-1,sizeof(dp));
    cout<<min(dfs(1,0),dfs(1,1));
    return 0;
}

int dfs(int now,int isInclude){
    int& result=dp[now][isInclude];
    if(result!=-1)return result;
    chk[now]=true;
    result=isInclude;
    for(int next=0;next<v[now].size();next++){
        if(chk[v[now][next]])continue;
        if(isInclude==0)result+=dfs(v[now][next],1);
        else result+=min(dfs(v[now][next],0),dfs(v[now][next],1));
    }
    chk[now]=false;
    return result;
}
```
<br>

### 참고 문제
* <a href="https://www.acmicpc.net/problem/1005">1005번 ACM Craft</a>
* <a href="https://www.acmicpc.net/problem/1949">1949번 우수마을</a>
* <a href="https://www.acmicpc.net/problem/15681">15681번 트리와 쿼리</a>
* <a href="https://www.acmicpc.net/problem/12978">12978번 스크루지 민호 2</a>

---
## 관련 게시물
<a href="https://kimdahui42.github.io/categories/Algorithms/">알고리즘</a>

