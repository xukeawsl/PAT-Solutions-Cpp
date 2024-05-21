注意点：

1. 打印结果时的格式最好复制题目上的
2. 最短距离从合法的循环路径中取最短的

```cpp
#include <iostream>
#include <cstring>
#include <vector>
#include <cstdio>

using namespace std;

const int INF = 0x3f3f3f3f;

int g[201][201];
bool vis[201];

int main() {
    int n, m, k;

    memset(g, 0x3f, sizeof(g));

    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        g[a][b] = g[b][a] = min(g[a][b], c);
    }

    cin >> k;

    int ans = INF;
    int sp = -1;
    
    for (int p = 1; p <= k; p++) {
        int cnt;
        cin >> cnt;
        vector<int> vec(cnt);
        for (int i = 0; i < cnt; i++) {
            cin >> vec[i];
        }

        memset(vis, 0, sizeof(vis));

        bool iscycle = false;
        bool issimple = false;
        bool isNa = false;
        if (vec[0] == vec[cnt - 1]) iscycle = true;
        if (iscycle && cnt == n + 1) issimple = true;

        int total = 0;
        for (int i = 1; i < cnt; i++) {
            int u = vec[i], v = vec[i - 1];
            vis[u] = vis[v] = true;
            if (g[u][v] == INF) {
                isNa = true;
                break;
            }
            total += g[u][v];
        }

        if (isNa) {
            printf("Path %d: NA (Not a TS cycle)\n", p);
            continue;
        }

        for (int i = 1; i <= n; i++) {
            if (!vis[i]) {
                iscycle = false;
            }
        }

        if (!iscycle) {
            printf("Path %d: %d (Not a TS cycle)\n", p, total);
        } else {
            if (total < ans) {
                ans = total;
                sp = p;
            }
            if (issimple) {
                printf("Path %d: %d (TS simple cycle)\n", p, total);
            } else {
                printf("Path %d: %d (TS cycle)\n", p, total);
            }
        }
    }

    printf("Shortest Dist(%d) = %d\n", sp, ans);
    
    return 0;
}
```
