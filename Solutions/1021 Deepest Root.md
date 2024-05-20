注意点：

1. 以每个节点为根求树的高度，1e4 的节点数 n^2 可以过不会超时

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>
#include <cstdio>

using namespace std;

int n;
vector<bool> vis;
vector<vector<int>> graph;

void dfs(int u) {
    vis[u] = true;

    for (auto v : graph[u]) {
        if (vis[v]) continue;
        dfs(v);
    }
}

int main() {
    cin >> n;

    vis = vector<bool>(n + 1);
    graph = vector<vector<int>>(n + 1);
    
    for (int i = 0; i < n + 1; i++) {
        int a, b;
        cin >> a >> b;
        graph[a].push_back(b);
        graph[b].push_back(a);
    }

    int cnt = 0;
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            dfs(i);
            cnt++;
        }
    }

    if (cnt > 1) {
        printf("Error: %d components\n", cnt);
        return 0;
    }

    int maxd = -1;
    vector<int> deep(n + 1);
    for (int i = 1; i <= n; i++) {
        fill(vis.begin(), vis.end(), false);
        queue<int> q;
        q.push(i);
        vis[i] = true;
        int d = 1;
        while (!q.empty()) {
            int m = q.size();
            for (int i = 0; i < m; i++) {
                int u = q.front(); q.pop();
                for (int v : graph[u]) {
                    if (!vis[v]) {
                        q.push(v);
                        vis[v] = true;
                    }
                }
            }
            d++;
        }

        maxd = max(maxd, d);
        deep[i] = d;
    }

    for (int i = 1; i <= n; i++) {
        if (deep[i] == maxd) {
            cout << i << endl;
        }
    }
    
    return 0;
}


```
