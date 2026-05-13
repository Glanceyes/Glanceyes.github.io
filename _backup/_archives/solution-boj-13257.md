---
title: "[Solution] BOJ 13257"
featured_image: "/images/thumbnail/2023-09-01-01.png"
tags:
- BOJ
- algorithm
date: '2023-09-01 17:00:00'
---

![img](https://vip2.loli.io/2023/09/01/GrQXC24wTZ6te5H.png)



<br/>

## BOJ 백준 13257 생태학



**문제: [https://www.acmicpc.net/problem/13257](https://www.acmicpc.net/problem/13257)**

<br/>

첫째 줄에 N, C, D, M이 주어진다. ($1 ≤ N ≤ 20$, $1 ≤ C ≤ 20$, $1 ≤ D ≤ 5$, $0 ≤ M ≤ N$)

$D$일 동안 매일마다 $C$ 마리를 포획하여 측정기가 부착이 안 된 새에 모두 측정기를 부착한다고 한다.

새가 총 $N$ 마리일 때, $D$일 후 $M$ 마리가 될 확률을 구하는 것이 문제이다.



<br/>

처음에 문제를 봤을 때는 $N$, $C$, $D$, $M$의 크기가 작은 편이어서 브루스포스로 구하는 단순 확률 문제인 줄 알았으나, 날마다 C마리의 새를 포획했을 때 몇 마리가 이미 측정기가 부착되었는지를 고려해야 하므로 생각보다 경우의 수가 많음을 깨달았다.

그래서 다이나믹 프로그래밍(Dynamic Programming)으로 이전 상태를 저장하여 다음 날의 확률을 구하는 방법으로 접근했다.

다이나믹 프로그래밍은 항상 메모이제이션할 배열을 잘 정의하는 것이 중요하다.

<br/>

$\text{dp}[i][j]$: 첫 번째부터 $i$번째 일까지 전체 새 중에서 $j$ 마리가 이미 측정기가 부착되었을 확률

<br/>

$i$번째 일에 $C$ 마리를 포획했을 때 얼만큼의 새가 이미 측정기가 부착되었느냐에 따라 $i+1$번째 일의 결과에 영향을 끼칠 것이다.

$C$ 마리를 포획한 새 중에서 부착되어 있지 않은 새에 모두 측정기를 부착한다고 했으므로, $C$ 마리 중에 $k$ 마리가 부착되어 있으면 $C - k$ 마리는 무조건 측정기가 부착되어 $i + 1$번째 일에는 $j + C - k$ 마리의 새가 측정기가 부착된 상태일 것이다.

<br/>

그러면 $N$ 마리에서 $C$ 마리를 포획했을 때 $k$ 마리가 이미 측정기가 부착되었을 확률을 구하려면, (1) 부착되어 있는 전체 $j$ 마리의 새 중에서 $k$ 마리를 포획하고, (2) 그렇지 않은 나머지 $N - j$ 마리의 새 중에서 $C - k$ 마리를 포획하는 것과 같다.

(1)과 (2)가 동시에 일어나는 경우의 수를 구하는데, 이 둘은 독립 사건이므로 사건의 교집합을 구할 때 서로 곱 연산을 취해준다.

이를 정리하면 다음과 같이 식을 나타낼 수 있다.

<br/>

$$ \text{dp}[i + 1][j + C - k] = \text{dp}[i][j] \times \frac{\binom{j}{k} \binom{N - j}{C - k}}{\binom{N}{C}} $$

<br/>

추가적으로 조합을 구하는 것도 고려해야 하는데, 다행히 문제에서 $N$, $C$, $M$가 충분히 작아서 다이나믹 프로그래밍으로 조합의 값을 구하는 데는 큰 문제가 없다.

다음과 같은 조합의 성질을 이용하면 된다.

<br/>

$$ \binom{N}{k} = \binom{N - 1}{k - 1} +  \binom{N - 1}{k} $$

<br/>

이는 다이나믹 프로그래밍 식을 잘 세워서 구할 수 있는데, 조합론 문제에 자주 나오는 내용이어서 정리해 두는 게 필요하다.

<br/>

$$ \text{comb}[n][k] = \text{comb}[n - 1][k - 1] + \text{comb}[n - 1][k] $$

<br/>

```C++
#include <iostream>
#include <algorithm>
using namespace std;

const int N_MAX = 20;
const int D_MAX = 5;

int N, C, D, M;
double comb[N_MAX * 2 + 1][N_MAX * 2 + 1];
double dp[D_MAX + 1][N_MAX + 1];

int main(){
    cin.tie(NULL);
    cout.tie(NULL);
    ios_base::sync_with_stdio(false);
    
    cin >> N >> C >> D >> M;
    
    for (int i = 0; i <= N; i++){
        comb[i][0] = comb[i][i] = 1;
    }
    
    for (int i = 1; i <= N; i++){
        for (int j = 1; j <= i; j++){
            comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j];
        }
    }
    
    dp[0][0] = 1;
    
    for (int i = 0; i < D; i++){
        for (int j = 0; j <= M; j++){
            if (dp[i][j] == 0) continue;
            for (int k = 0; k <= j && k <= C; k++){
                dp[i + 1][j + C - k] += dp[i][j] * (comb[j][k] * comb[N - j][C - k]) / comb[N][C];
            }
        }
    }
    
    cout << fixed;
    cout.precision(20);
    
    cout << dp[D][M] << "\n";
    
    return 0;
}
```
