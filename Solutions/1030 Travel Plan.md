注意点：

1. 最短距离相等的情况下，选择 cost 最小的路径

```cpp
#include <iostream>
#include <cstring>
#include <vector>

using namespace std;

int g[501][501];
int c[501][501];
int dist[501], cost[501];
bool vis[501];
vector<int> ans, path;
int n, m, s, d;

const int INF = 0x3f3f3f3f;

void djkstra(int s) {
    memset(vis, 0, sizeof(vis));
    memset(dist, 0x3f, sizeof(dist));
    memset(cost, 0x3f, sizeof(cost));

    dist[s] = 0;
    cost[s] = 0;

    for (int i = 0; i < n; i++) {
        int u = -1, mind = INF;
        for (int j = 0; j < n; j++) {
            if (!vis[j] && dist[j] < mind) {
                mind = dist[j];
                u = j;
            }
        }
        if (u == -1) break;
        vis[u] = true;

        for (int v = 0; v < n; v++) {
            if (!vis[v] && g[u][v] != INF) {
                if (dist[v] == dist[u] + g[u][v]) {
                    cost[v] = min(cost[v], cost[u] + c[u][v]);
                } else if (dist[v] > dist[u] + g[u][v]) {
                    cost[v] = cost[u] + c[u][v];
                    dist[v] = dist[u] + g[u][v];
                }
            }
        }
    }
}

bool dfs(int u, int len, int cos) {
    if (len > dist[d]) return false;
    if (len == dist[d]) {
        if (cos == cost[d]) {
            ans = path;
            return true;
        }
        return false;
    }

    for (int v = 0; v < n; v++) {
        if (!vis[v] && g[u][v] != INF) {
            vis[v] = true;
            path.push_back(v);
            if (dfs(v, len + g[u][v], cos + c[u][v])) {
                return true;
            }
            vis[v] = false;
            path.pop_back();
        }
    }

    return false;
}

int main() {
    cin >> n >> m >> s >> d;

    memset(g, 0x3f, sizeof(g));
    memset(c, 0x3f, sizeof(c));

    for (int i = 0; i < n; i++) g[i][i] = 0;

    for (int i = 0; i < m; i++) {
        int c1, c2, dis, cos;
        cin >> c1 >> c2 >> dis >> cos;
        g[c1][c2] = g[c2][c1] = min(g[c1][c2], dis);
        c[c1][c2] = c[c2][c1] = min(c[c1][c2], cos);
    }

    djkstra(s);

    memset(vis, 0, sizeof(vis));
    vis[s] = true;
    path.push_back(s);
    dfs(s, 0, 0);

    for (int x : ans) {
        cout << x << ' ';
    }

    cout << dist[d] << ' ' << cost[d] << endl;
    
    return 0;
}
```
