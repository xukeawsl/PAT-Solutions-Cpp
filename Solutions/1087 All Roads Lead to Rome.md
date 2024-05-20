注意点：

1. 统计路径数的时候，如果遇到相同距离的情况，应该是叠加计算而不是简单的加一

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <string>
#include <cstring>

using namespace std;

const int INF = 0x3f3f3f3f;

int n, k;
string start;

int genid;
unordered_map<int, string> cityName;
unordered_map<string, int> cityId;

unordered_map<int, int> happiness;

int g[201][201];
int dist[201];
int w[201], cnt[201];
bool st[201];
vector<int> ans, path;
int mincnt;

void dfs(int u, int ed, int tcost,int hpy, int count) {
    if (tcost > dist[ed]) return;
    if (u == ed) {
        if (tcost == dist[ed] && w[ed] == hpy) {
            if (ans.empty()) {
                mincnt = count;
                ans = path;
            } else if (count < mincnt) {
                mincnt = count;
                ans = path;
            }
        }
        return;
    }

    for (int v = 0; v < n; v++) {
        if (!st[v] && g[u][v] != INF) {
            st[v] = true;
            path.emplace_back(v);
            dfs(v, ed, tcost + g[u][v], hpy + happiness[v], count + 1);
            path.pop_back();
            st[v] = false;
        }
    }
}

void dijkstra(int s) {
    memset(dist, 0x3f, sizeof(dist));
    memset(st, 0, sizeof(st));
    dist[s] = 0;

    for (int i = 0; i < n - 1; i++) {
        int u = -1, mind = INF;
        for (int j = 0; j < n; j++) {
            if (!st[j] && dist[j] < mind) {
                mind = dist[j];
                u = j;
            }
        }
        if (u == -1) break;

        st[u] = true;

        for (int v = 0; v < n; v++) {
            if (!st[v] && g[u][v] != INF) {
                if (dist[v] == dist[u] + g[u][v]) {
                    if (w[v] < w[u] + happiness[v]) {
                        w[v] = w[u] + happiness[v];
                    }
                    cnt[v] += cnt[u];
                } else if (dist[v] > dist[u] + g[u][v]) {
                    dist[v] = dist[u] + g[u][v];
                    w[v] = w[u] + happiness[v];
                    cnt[v] = cnt[u];
                }
            }
        }
    }
}

int main() {
    cin >> n >> k >> start;
    cityName[genid] = start;
    cityId[start] = genid;
    happiness[genid] = 0;
    genid++;
    for (int i = 0; i < n - 1; i++) {
        string city;
        int value;
        cin >> city >> value;
        cityName[genid] = city;
        cityId[city] = genid;
        happiness[genid] = value;
        genid++;
    }

    memset(g, 0x3f, sizeof(g));
    for (int i = 0; i < n; i++) g[i][i] = 0;

    for (int i = 0; i < k; i++) {
        string c1, c2;
        int cost;
        cin >> c1 >> c2 >> cost;
        int u = cityId[c1], v = cityId[c2];
        g[u][v] = g[v][u] = min(g[u][v], cost);
    }

    cnt[0] = 1;
    dijkstra(0);

    int ed = cityId["ROM"];
    cout << cnt[ed] << ' ' << dist[ed] << ' ' << w[ed] << ' ';

    memset(st, 0, sizeof(st));
    st[0] = true;
    path.emplace_back(0);
    dfs(0, ed, 0, 0, 0);

    cout << w[ed] / mincnt << endl;

    for (int i = 0; i < ans.size(); i++) {
        cout << cityName[ans[i]];
        if (i != ans.size() - 1) cout << "->";
        else cout << endl;
    }
    
    return 0;
}
```
