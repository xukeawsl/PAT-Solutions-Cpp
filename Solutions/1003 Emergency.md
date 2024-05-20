注意点：统计最短路径数的时候需要在出现相同距离情况的时候累加，不需要求每一条具体的路径

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int INF = 0x3f3f3f3f;

int main() {
    int count[501] = {0};
    int g[501][501];
    int dist[501];
    int pc[501] = {0}, w[501] = {0}; // 路径数和救援队数
    bool vis[501] = {false};
    int n, m, c1, c2;
    
    memset(g, 0x3f, sizeof(g));
    memset(dist, 0x3f, sizeof(dist));
    
    cin >> n >> m >> c1 >> c2;

    for (int i = 0; i < n; i++) {
        cin >> count[i];
    }

    for (int i = 0; i < m; i++) {
        int x, y, l;
        cin >> x >> y >> l;
        g[x][y] = g[y][x] = l;
    }

    dist[c1] = 0;
    pc[c1] = 1;
    w[c1] = count[c1];
    
    for (int i = 0; i < n; i++) {
        int u = -1, min_dist = INF;
        for (int k = 0; k < n; k++) {
            if (dist[k] < min_dist && !vis[k]) {
                u = k;
                min_dist = dist[k];
            }
        }
            
        vis[u] = true;
        for (int v = 0; v < n; v++) {
            if (g[u][v] != INF) {
                int d = dist[u] + g[u][v];
                if (dist[v] == d) {
                    pc[v] += pc[u];
                    w[v] = max(w[v], w[u] + count[v]);
                } else if (dist[v] > d) {
                    pc[v] = pc[u];
                    dist[v] = d;
                    w[v] = w[u] + count[v];
                }
            }
        }
    }

    cout << pc[c2] << ' ' << w[c2] << endl;
    
    return 0;
}
```
