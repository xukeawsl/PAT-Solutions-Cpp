注意点：

1. 求给定两点的最短经过站点数可以想到用最短路，如果边权为 1 可以用 bfs，但是本题还要求当存在相同长路时换乘的次数最少，因此需要根据换乘的特点建图
2. 对于给定的一条线路，我们将其中任意两点间建立一条边，含义就是从点 u 到 v 或点 v 到点 u 之后会进行换乘，边权的含义则是两点间要经过多少个站点，这样问题就被转换为求边权和最短的路径，且路径相同时取边数最少的模型了
3. 由于还需要记录线路，因此我们可以在求最短路的时候维护从上一个节点到达当前节点的信息以及从哪个节点到达当前节点最优，最后从终点反求到起点即可得到具体路径
4. 由于线路可能存在环，即 A->B->A 这种情况，因此在计算边权的时候要取环两种情况的最短距离
5. 由于边数很多，需要使用堆优化的 dijkstra 算法，注意退出条件是终点已经被处理过，因为一个节点可能属于多条线路，因此不能和一般的做法那样，同一个节点可能要松弛多次

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <tuple>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <string>

using namespace std;

vector<vector<tuple<int, int, int>>> graph(10000);
vector<int> stops(10000);
vector<string> route(10000);

int dist[10000], cnt[10000], pre[10000];
bool vis[10000];

string getroute(int l, int u, int v) {
    char buf[200];
    sprintf(buf, "Take Line#%d from %04d to %04d.", l, u, v);
    return buf;
}

void dijkstra(int st, int ed) {
    memset(dist, 0x3f, sizeof(dist));
    memset(cnt, 0x3f, sizeof(cnt));
    memset(vis, 0, sizeof(vis));

    dist[st] = cnt[st] = 0;

    using PII = pair<int, int>;
    priority_queue<PII, vector<PII>, greater<PII>> pq;
    pq.emplace(dist[st], st);
    
    while (!pq.empty()) {
        auto u = pq.top().second; pq.pop();
        if (u == ed) break;
        if (vis[u]) continue;
        vis[u] = true;

        for (auto [v, d, l] : graph[u]) {
            if (dist[v] > dist[u] + d) {
                dist[v] = dist[u] + d;
                cnt[v] = cnt[u] + 1;
                pre[v] = u;
                pq.emplace(dist[v], v);
                route[v] = getroute(l, u, v);
            } else if (dist[v] == dist[u] + d && cnt[v] > cnt[u] + 1) {
                cnt[v] = cnt[u] + 1;
                pre[v] = u;
                route[v] = getroute(l, u, v);
            }
        }
    }

    cout << dist[ed] << endl;
    vector<string> path;
    for (int i = ed; i != st; i = pre[i]) {
        path.emplace_back(route[i]);
    }
    reverse(path.begin(), path.end());
    for (auto& p : path) {
        cout << p << endl;
    }
}

int main() {
    int n, m, k;
    cin >> n;
    
    for (int i = 1; i <= n; i++) {
        cin >> m;
        for (int j = 1; j <= m; j++) {
            cin >> stops[j];
        }
        
        for (int j = 1; j <= m; j++) {
            for (int k = 1; k < j; k++) {
                int d = j - k;
                if (stops[1] == stops[m]) d = min(d, m - 1 - (j - k));

                graph[stops[j]].emplace_back(stops[k], d, i);
                graph[stops[k]].emplace_back(stops[j], d, i);
            }
        }
    }
    
    cin >> k;
    while (k--) {
        int start, end;
        cin >> start >> end;
        dijkstra(start, end);
    }
    
    return 0;
}
```
