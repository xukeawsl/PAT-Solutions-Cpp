注意点：

1. 两轮 djkstra 求最短距离路径和时间路径，对于总距离相同的距离路径，取耗时更短的一条，对于总耗时相同的时间路径，取经过交叉点最少的路径
2. 对于输入的数据，有单向边也有双向边，注意要考虑重边的问题，重边取最短的一条即可

```cpp
#include <iostream>
#include <cstdio>
#include <vector>
#include <cstring>

using namespace std;

int g[501][501], t[501][501];
int dist[501], mt[501];
bool vis[501];
int n, m;
int source, dest;

vector<int> distp, timep;
int mint, minc;

vector<int> path;

const int INF = 0x3f3f3f3f;

void getPathD(int u, int total_dist, int total_time) {
    if (total_dist == dist[dest]) {
        if (u != dest) return;
        if (distp.empty()) {
            distp = path;
            mint = total_time;
        } else {
            if (total_time < mint) {
                mint = total_time;
                distp = path;
            }
        }
        return;
    }

    for (int v = 0; v < n; v++) {
        if (!vis[v] && g[u][v] != INF && total_dist + g[u][v] <= dist[dest]) {
            vis[v] = true;
            path.emplace_back(v);
            getPathD(v, total_dist + g[u][v], total_time + t[u][v]);
            path.pop_back();
            vis[v] = false;
        }
    }
}

void getPathT(int u, int total_time, int cnt) {
    if (total_time == mt[dest]) {
        if (u != dest) return;
        if (timep.empty()) {
            timep = path;
            minc = cnt;
        } else {
            if (cnt < minc) {
                minc = cnt;
                timep = path;
            }
        }
        return;
    }

    for (int v = 0; v < n; v++) {
        if (!vis[v] && t[u][v] != INF && total_time + t[u][v] <= mt[dest]) {
            vis[v] = true;
            path.emplace_back(v);
            getPathT(v, total_time + t[u][v], cnt + 1);
            path.pop_back();
            vis[v] = false;
        }
    }
}

void dijkstra(int s, bool flag) {
    if (flag) memset(dist, 0x3f, sizeof(dist));
    else memset(mt, 0x3f, sizeof(mt));
    memset(vis, 0, sizeof(vis));

    if (flag) dist[s] = 0;
    else mt[s] = 0;

    for (int i = 0; i < n; i++) {
        int u = -1, minval = INF;
        for (int j = 0; j < n; j++) {
            if (flag) {
                if (!vis[j] && dist[j] < minval) {
                    minval = dist[j];
                    u = j;
                }
            } else {
                if (!vis[j] && mt[j] < minval) {
                    minval = mt[j];
                    u = j;
                }
            }
        }
        if (u == -1) break;
        vis[u] = true;

        for (int v = 0; v < n; v++) {
            if (flag) {
                if (!vis[v] && g[u][v] != INF) {
                    dist[v] = min(dist[v], dist[u] + g[u][v]);
                }
            } else {
                if (!vis[v] && t[u][v] != INF) {
                    mt[v] = min(mt[v], mt[u] + t[u][v]);
                }
            }
        }
    }
}

int main() {
    cin >> n >> m;

    memset(g, 0x3f, sizeof(g));
    memset(t, 0x3f, sizeof(t));
    for (int i = 0; i < n; i++) g[i][i] = 0;

    for (int i = 0; i < m; i++) {
        int v1, v2, one_way, length, time;
        cin >> v1 >> v2 >> one_way >> length >> time;
        g[v1][v2] = min(g[v1][v2], length);
        t[v1][v2] = min(t[v1][v2], time);
        if (one_way == 0) {
            g[v2][v1] = min(g[v2][v1], length);
            t[v2][v1] = min(t[v2][v1], time);
        }
    }

    cin >> source >> dest;
    
    dijkstra(source, true);
    dijkstra(source, false);

    memset(vis, 0, sizeof(vis));
    vis[source] = true;
    getPathD(source, 0, 0);
    memset(vis, 0, sizeof(vis));
    vis[source] = true;
    getPathT(source, 0, 0);

    if (distp == timep) {
        cout << "Distance = " << dist[dest] << "; Time = " << mt[dest] << ": "
             << source;
        for (auto u : distp) cout << " -> " << u;
        cout << '\n';
    } else {
        cout << "Distance = " << dist[dest] << ": " << source;
        for (auto u : distp) cout << " -> " << u;
        cout << '\n';
        cout << "Time = " << mt[dest] << ": " << source;
        for (auto u : timep) cout << " -> " << u;
        cout << '\n';
    }
    
    return 0;
}
```
