注意点：

1. 题目要求判断的不是给定的序列是否是拓扑排序可能的序列，而是给定的序列能否以拓扑序访问，也就是说依次访问，每次访问的节点的入读都要为 0，当访问一个节点后，所有它的邻接点的入度都要更新

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int n, m, k;
vector<vector<int>> graph(1001);
vector<int> indegree(1001);

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        graph[u].emplace_back(v);
        indegree[v]++;
    }

    cin >> k;

    vector<int> ans;
    for (int i = 0; i < k; i++) {
        bool flag = true;
        vector<int> tmp = indegree;
        for (int j = 0; j < n; j++) {
            int u;
            cin >> u;

            if (tmp[u] != 0) {
                flag = false;
            }

            if (!flag) continue;

            for (int v : graph[u]) tmp[v]--;
        }

        if (!flag) ans.emplace_back(i);
    }

    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i];
        if (i != ans.size() - 1) cout << ' ';
        else cout << '\n';
    }
    
    return 0;
}
```
