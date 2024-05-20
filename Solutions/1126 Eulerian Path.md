注意点：

1. 欧拉图需要满足连通图且每个点的度数都为偶数，半欧拉图则是连通图中恰好有两个点的度为奇数，剩余的是非欧拉图

```cpp
#include <iostream>

using namespace std;

int degree[501];
bool g[501][501];
bool vis[501];
int n, m;

int dfs(int u) {
    vis[u] = true;
    int cnt = 1;

    for (int v = 1; v <= n; v++) {
        if (!vis[v] && g[u][v]) {
            cnt += dfs(v);
        }
    }

    return cnt;
}

int main() {
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        degree[a]++;
        degree[b]++;
        g[a][b] = g[b][a] = true;
    }

    bool isPath = (dfs(1) == n);
    bool flag = true;
    int odd = 0;
    for (int i = 1; i <= n; i++) {
        if (degree[i] && degree[i] % 2) {
            flag = false;
            odd++;
        }
        cout << degree[i];
        if (i != n) cout << ' ';
        else cout << '\n';
    }

    if (isPath && flag) {
        cout << "Eulerian" << endl;
    } else {
        if (isPath && odd == 2) {
            cout << "Semi-Eulerian" << endl;
        } else {
            cout << "Non-Eulerian" << endl;
        }
    }
    
    return 0;
}
```
