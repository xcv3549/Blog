---
title: "[BOJ 12100] 2048(Easy)"
date: 2020-04-26T10:37:29Z
description: 백준 12100번 풀이
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: outer
tags: ["BOJ"]
categories: ["algorithm"]
---


### [<문제 바로가기>](https://www.acmicpc.net/problem/12100)


---

### 풀이

#### 접근 포인트
######
1. 1개의 상태에서 4개(상하좌우)의 상태로 확장 가능하다.
	- 모든 상태의 수 : 4<sup>5</sup>
	- 각 상태의 마지막 단계에서 최대 블록을 만들 수 있다고 가정한다.
	- __DFS를 이용하여 모든 상태를 방문한다.__

2. 상-하 , 좌-우는 서로 정반대의 동작이기 때문에 구현도 정반대로 이루어진다.
######
---

### 소스코드


<details><summary>보기</summary>

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int solve(int cnt , int Map[][20]);

int N;
int Map[20][20];

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(nullptr);

	int i,j,ret;

	cin >> N;
	for(i=0; i<N; i++)
		for(j=0; j<N; j++)
			cin >> Map[i][j];

	cout << solve(0,Map) << endl;

}

int solve(int cnt , int Map[][20])
{
	int i,j,k,dir,val,ret=0;
	int Array[20][20] = {0,};

	if(cnt == 5)
	{
		for(i=0; i<N; i++)
			for(j=0; j<N; j++)
				ret = max(ret , Map[i][j]);

		return ret;
	}

	for(dir=0; dir<4; dir++)
	{
		for(i=0; i<N; i++)
		{
			vector<int> line;
			for(j=0; j<N; j++)
			{
				val = (dir<2 ? Map[i][j] : Map[j][i]);
				if(val != 0)	line.push_back(val);
			}

			if(dir == 1 || dir == 3)
				reverse(line.begin(),line.end());

			vector<int> mix_line;
			for(k=0; k<line.size(); k++)
			{
				if(k+1 < line.size() && line[k+1] == line[k])
				{
					mix_line.push_back(line[k]*2);
					k++;
				}

				else
					mix_line.push_back(line[k]);
			}

			for(k=mix_line.size(); k<N; k++)
				mix_line.push_back(0);

			if(dir == 1 || dir == 3)
				reverse(mix_line.begin(),mix_line.end());

			for(j=0; j<N; j++)
				(dir<2 ? Array[i][j] : Array[j][i]) = mix_line[j];
		}

		ret = max(ret , solve(cnt+1 , Array));
	}

	return ret;
}

```

</details>