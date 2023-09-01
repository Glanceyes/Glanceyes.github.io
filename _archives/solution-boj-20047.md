---
title: "[Solution] BOJ 20047"
featured_image: "/images/thumbnail/2023-09-01-02.png"
tags:
- BOJ
- algorithm
date: '2023-09-01 21:00:00'
---



![](https://vip2.loli.io/2023/09/01/J3wGuztndfRhZcT.png)

<br/>

## BOJ 백준 20047 동전 옮기기

**문제: [https://www.acmicpc.net/problem/20047](https://www.acmicpc.net/problem/20047)**

<br/>

두 개의 동전을 서로 순서를 바꾸지 않고 자리를 이동하여 문제에서 주어지는 동전 배치를 만들 수 있는지 묻는 문제이다.

학회에서 ACM ICPC 예선을 준비하면서 팀원과 같이 풀었던 문제이다. 처음에는 Queue를 이용하여 푸는 구현 문제인 줄 알고 시도했는데, 계속 채점을 돌려봐도 86퍼센트에서 틀렸다고 떠서 접근 자체가 틀렸음을 직감했다. 뒤늦게 다시 풀어봤는데 결국 **다이나믹 프로그래밍(Dynamic Programming)**으로 풀 수 있는 문제였다.

<br/>

메모이제이션 할 배열 dp를 아래와 같이 정의했다.



<br/>

$dp[i][j]$: $i$번째 원소까지 봤을 때 이동해야 하는 동전을 $j$개 이동하여 문자열 $T[0:i]$를 만들 수 있는지 여부

<br/>



여기서 $T[0:i]$는 주어지는 동전 배치 문자열 $T$의 0번째부터 $i$번째까지의 부분문자열이다.

<br/>

구현의 편의를 위해 동전을 이동하지 않고 제자리에 가만히 두는 경우도 해당 동전을 자신의 자리로 이동하는 것으로 전제한다. 즉, 문자열 $S$의 모든 원소를 다 봤을 때 이동했어야 하는 동전은 무조건 2개여야 한다.

그런데 이동하기 전의 기존 문자열 $S$에서 이동하는 동전 두 개에 대응하는 문자를 제외한 새로운 문자열 $S$’를 가지고 문자열 $T$와 비교하며 배열 $dp$를 업데이트 해야 하는데, 이렇게 고려하지 않고 단순히 문자열 $S$를 가지고 비교하면 이동해야 하는 동전 두 개에 대한 처리가 곤란해진다. 위의 정리 노트의 예를 보면 이해하기 쉽다.

<br/>

배열 $dp$의 정의를 구현상 좀 더 쉽게 바꾸면 다음과 같다.



<br/>



$Two$: 이동해야 하는 동전 두 개에 대한 문자열



<br/>



$dp[i][status]$: 문자열 $T$의 $i$번째 원소까지 봤을 때 동전한 이동 상태가 $status$인 경우가 가능한지



<br/>

$status$가 0인 경우: $Two$의 아무런 동전 이동하지 않은 상태



$status$가 1인 경우: $Two[0]$(첫 번째 동전)만 이동한 상태



$status$가 2인 경우: $Two[0]$과 $Two[1]$(첫 번째와 두 번째 동전) 모두 이동한 상태



<br/>

<br/>



즉, 관점을 바꿔서 생각해보면 문자열 $S’$에 $Two$의 원소를 순서에 맞도록 알맞게 배치하여 문자열 $T$를 만들 수 있는지를 판단하는 문제가 된다. 문자열 $S’$와 $T$ 각각을 가리키는 인덱스 $idx1$와 $idx2$를 이동하면서 탐색하는 Top-Down 방식의 함수를 구현했다.

<br/>

### 1\. $T[idx2]$에 $Two[status]$를 배치하는 경우

<br/>

$T[idx2] == Two[status]$이면

→ $T[idx2]$에 $Two[status]$를 배치하고 $S’[idx1]$와 $T[idx2 + 1]$ 이어서 비교



<br/>
$$
dp[idx2][status] \mathrel{|}= go(idx1, idx2 + 1, status + 1)
$$




<br/>

### 2\. $T[idx2]$에 배치하지 않는 경우

<br/>

$S’[idx1] == T[idx2]$이면

→ $S’[idx1 + 1]$와 $T[idx2 + 1]$ 이어서 비교



<br/>
$$
dp[idx2][status] \mathrel{|}= go(idx1 + 1, idx2 + 1, status)
$$






<br/>



### 3\. Base Case 

<br/>

$idx2 == n$일 때

$status == 2$이면(동전 두 개 다 배치한 경우) → 1 반환

$status < 2$이면 → 0 반환



<br/>



```C++
#include <iostream>
#include <cstring>
#include <string.h>
#include <algorithm>
using namespace std;

const int N_MAX = (int)1e4;

int n, l, r;
int dp[N_MAX][3];
string s, t, two;

int go(int idx1, int idx2, int status){
    int& ret = dp[idx2][status];
    if (ret != -1){
        return ret;
    }
    
    if (idx2 == n){
        if (status == 2){
            return ret = 1;
        }
        return ret = 0;
    }
    
    ret = 0;
    
    if (status < 2){
        if (t[idx2] == two[status]){
            ret |= go(idx1, idx2 + 1, status + 1);
        }
    }
    
    if (s[idx1] == t[idx2]){
        if (idx1 < n - 2){
            ret |= go(idx1 + 1, idx2 + 1, status);
        }
    }

    return ret;
}

int main(){
    cin.tie(NULL);
    cin.sync_with_stdio(false);
    
    memset(dp, -1, sizeof(dp));
    
    string x;
    cin >> n >> x >> t >> l >> r;

    two += x[l];
    two += x[r];

    for (int i = 0; i < n; i++){
        if (i != l && i != r){
            s += x[i];
        }
    }
    
    if (go(0, 0, 0)){
        cout << "YES\n";
    }
    else {
        cout << "NO\n";
    }
    
    return 0;
}
```
