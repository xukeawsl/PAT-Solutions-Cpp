注意点：

1. 多退少补，如果大于 c / 2 ，则多余的部分可以给路径上其它更少的节点使用，但是要按照路径顺序来，不能统计总和来计算
2. 当有两个满足发送自行车数量最少的最短路径，则选择回收数量较少的一条路径
3. 使用回溯法来求每条最短路径，可以通过距离来剪枝

```cpp
#include <iostream>
#include <cstring>
#include <vector>

using namespace std;

int cnt[501];
int dist[501];
bool vis[501];
int g[501][501];

int c, n, p, m;

const int INF = 0x3f3f3f3f;

void djkstra(int s) {
    memset(dist, 0x3f, sizeof(dist));
    dist[s] = 0;

    for (int i = 0; i <= n; i++) {
        int u = -1, mind = INF;
        for (int j = 0; j <= n; j++) {
            if (!vis[j] && dist[j] < mind) {
                mind = dist[j];
                u = j;
            }
        }
        if (u == -1) break;
        vis[u] = true;

        for (int v = 0; v <= n; v++) {
            if (!vis[v] && g[u][v] != INF) {
                dist[v] = min(dist[v], dist[u] + g[u][v]);
            }
        }
    }
}

vector<int> path;
vector<int> ans;
int mins = INF, rece = INF;

void dfs(int u, int p, int step, int sum, int send) {
    if (step > dist[p]) {
        return;
    }
    if (u == p) {
        if (send < mins) {
            mins = send;
            ans = path;
            rece = sum;
        } else if (mins == send && sum < rece) {
            ans = path;
            rece = sum;
        }
        return;
    }

    
    for (int i = 0; i <= n; i++) {
        if (vis[i]) continue;
        path.push_back(i);
        vis[i] = true;
        int nsend = send;
        int nsum = sum;
        if (cnt[i] > c / 2) {
            nsum += cnt[i] - c / 2;
        } else {
            if (nsum > (c / 2 - cnt[i])) {
                nsum -= c / 2 - cnt[i];
            } else {
                nsend += c /2 - cnt[i] - nsum;
                nsum = 0;
            }
        }
        dfs(i, p, step + g[u][i], nsum, nsend);
        path.pop_back();
        vis[i] = false;
    }
}

int main() {
    cin >> c >> n >> p >> m;

    memset(g, 0x3f, sizeof(g));
    for (int i = 0; i <= n; i++) g[i][i] = 0;

    for (int i = 0; i < n; i++) {
        cin >> cnt[i + 1];
    }

    for (int i = 0; i < m; i++) {
        int a, b, w;
        cin >> a >> b >> w;
        g[a][b] = g[b][a] = w;
    }

    djkstra(0);

    memset(vis, 0, sizeof(vis));
    vis[0] = true;
    dfs(0, p, 0, 0, 0);

    cout << mins << ' ';

    int u = 0;
    cout << u;
    for (int v : ans) {
        cout << "->" << v;
        u = v;
    }
    
    cout << ' ' << rece << endl;

    return 0;
}
```
