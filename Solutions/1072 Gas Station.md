注意点：

1. 题目要求建造的加油站与任何住宅之间的最小距离尽可能远，因此需要以每个加油站为源点做一次最短路算法
2. 加油站需要能够为所有房屋服务，且题目输入还限制了最大服务范围，即加油站与任意房屋的具体不能超过最大服务范围
3. 满足上述条件，返回离加油站最短的一个房屋的距离，以及到所有房屋的平均距离，最后在所有源点中取距离最远的，如果距离相同，则取平均距离更短的，如果还是相同，则取序号更小的
4. 注意返回的平均距离需要考虑精度问题，加上一个小数如 `1e-8` 保证正常进位

```cpp
#include <iostream>
#include <cstring>
#include <cstdio>

using namespace std;

int g[1011][1011];
int dist[1011];
bool vis[1011];
int n, m, k, d;

const int INF = 0x3f3f3f3f;

bool djkstra(int s, int& mindist, double& avgdist) {
    memset(vis, 0, sizeof(vis));
    memset(dist, 0x3f, sizeof(dist));
    dist[s] = 0;

    for (int i = 1; i <= n + m; i++) {
        int u = -1, mind = INF;
        for (int j = 1; j <= n + m; j++) {
            if (!vis[j] && dist[j] < mind) {
                mind = dist[j];
                u = j;
            }
        }
        if (vis[u]) break;
        vis[u] = true;

        for (int v = 1; v <= n + m; v++) {
            if (!vis[v] && g[u][v] != INF) {
                dist[v] = min(dist[v], dist[u] + g[u][v]);
            }
        }
    }

    int mind = INF;
    double avgd = 0.0;
    for (int i = 1; i <= n; i++) {
        if (dist[i] == INF) {
            return false;
        }

        if (dist[i] > d) {
            return false;
        }

        mind = min(mind, dist[i]);
        avgd += dist[i];
    }

    mindist = mind;
    avgdist = avgd / n + 1e-8;

    return true;
}

int main() {
    cin >> n >> m >> k >> d;

    memset(g, 0x3f, sizeof(g));
    for (int i = 1; i <= n + m; i++) g[i][i] = 0;

    for (int i = 0; i < k; i++) {
        string x, y;
        int u, v, z;
        cin >> x >> y >> z;

        if (x[0] == 'G') {
            u = n + stoi(x.substr(1));
        } else {
            u = stoi(x);
        }

        if (y[0] == 'G') {
            v = n + stoi(y.substr(1));
        } else {
            v = stoi(y);
        }
        
        g[u][v] = g[v][u] = min(g[u][v], z);
    }

    int station = -1;
    int maxd = -INF;
    double avgd = INF;
    
    for (int s = n + 1; s <= n + m; s++) {
        int dis;
        double avg;
        if (djkstra(s, dis, avg)) {
            if (dis > maxd) {
                station = s;
                maxd = dis;
                avgd = avg;
            } else if (dis == maxd && avg < avgd) {
                station = s;
                avgd = avg;
            }
        }
    }

    if (station == -1) {
        printf("No Solution");
    } else {
        printf("G%d\n%.1lf %.1lf", station - n, (double)maxd, avgd);
    }
    
    return 0;
}
```
