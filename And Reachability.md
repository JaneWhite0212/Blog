#### 题目大意

给定一个数列 $a$，我们称 $x$ 可以到达 $y$ 当且仅当：

1. $x < y$；
2. 存在一个数组 $p$，其中 $x=p_1 < p_2 \dots < p_k = y$，并且对于任意 $1 \leq i < k$，满足 $a_{p_i} \operatorname{AND} a_{p_{i+1}} > 0$。

现给出 $q$ 组下标，输出它们是否可以到达。

#### 思路

按位与操作可以按位考虑，$x$ 可以到达 $y$ 当且仅当 $x$ 与 $y$ 在二进制下某一位同时为 $1$ 或者 $x$ 与 $y$ 可以通过一些数中转到达。

那么设 $f_{i,j}$ 表示 $1 \sim i -1$ 位置第 $j$ 位上为 $1$ 的最靠后的位置，处理出这个用于后面的转移。

设 $dp_{i,j}$ 表示第 $1 \sim i-1$ 位置第 $j$ 位上为 $1$ 且能转移到 $a_i$ 的最靠后的位置，如果需要中转站，那么可以直接以 $f_{i,j}$ 为中转站转移，否则可以直接转移。

#### Code

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 300500;

int n,q;

int a[N];

int f[N][20],dp[N][20];

int main() {
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);

	cin >> n >> q;
	for(int i = 1;i <= n; i++) {
        cin >> a[i];

        for(int j = 0;j < 20; j++) {
            if((a[i - 1] >> j) & 1)
                f[i][j] = i - 1;
            else
                f[i][j] = f[i - 1][j];
        }
    }

    for(int i = 1;i <= n; i++) {
        for(int j = 0;j < 20; j++) {
            for(int k = 0;k < 20; k++) {
                if(!((a[i] >> k) & 1)) 
                    continue;

                dp[i][j] = max(dp[i][j],dp[f[i][k]][j]);

                if((a[f[i][k]] >> j) & 1)
                    dp[i][j] = max(dp[i][j],f[i][k]);
            }
        }
    }

	for(int i = 1,x,y;i <= q; i++) {
		cin >> x >> y;

        bool flag = 0;

        for(int j = 0;j < 20; j++) {
            if(((a[x] >> j) & 1) && dp[y][j] >= x) {
                flag = 1;
                break;
            }
        }

		if(flag)
			cout << "Shi\n";
		else
			cout << "Fou\n";
	}
	return 0;
}
```