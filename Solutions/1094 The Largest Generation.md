注意点：

1. 节点序号是从 1 开始的，因此数组大小要大一点
2. 实质上是求层序遍历数量最多的一层

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;


int main() {
    int n, m;

    cin >> n >> m;

    vector<vector<int>> graph(n + 1);

    for (int i = 0; i < m; i++) {
        int u, k, v;
        cin >> u >> k;
        for (int j = 0; j < k; j++) {
            cin >> v;
            graph[u].emplace_back(v);
        }
    }

    queue<int> q;
    q.emplace(1);

    int large_number = 0, large_level = 0;
    
    int level = 1;
    
    while (!q.empty()) {
        int cnt = q.size();
        if (cnt > large_number) {
            large_number = cnt;
            large_level = level;
        }

        for (int i = 0; i < cnt; i++) {
            int u = q.front(); q.pop();
            for (int v : graph[u]) {
                q.emplace(v);
            }
        }

        level++;
    }

    cout << large_number << ' ' << large_level << endl;
    
    return 0;
}
```
