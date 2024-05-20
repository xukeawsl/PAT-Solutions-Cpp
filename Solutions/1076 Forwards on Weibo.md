注意点：

1. 一个人发布消息时，订阅他的人可以转发，因此建图时，发布者应该指向订阅者
2. 题目指定了深度，则可以进行 BFS 一层一层的遍历并统计数量

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

int main() {
    int n, l, k;

    cin >> n >> l;

    vector<vector<int>> graph(n + 1);
    
    for (int i = 1; i <= n; i++) {
        int m;
        cin >> m;
        for (int j = 0; j < m; j++) {
            int user;
            cin >> user;
            graph[user].push_back(i);
        }
    }

    cin >> k;
    
    while (k--) {
        int query;
        cin >> query;

        vector<bool> vis(n + 1);
        queue<int> q;
        q.push(query);
        vis[query] = true;
        
        int level = l;
        int cnt = 0;
        while (!q.empty()) {
            int s = q.size();
            for (int i = 0; i < s; i++) {
                int u = q.front(); q.pop();
                for (int v : graph[u]) {
                    if (!vis[v]) {
                        cnt++;
                        vis[v] = true;
                        q.push(v);
                    }
                }
            }
            if (--level == 0) break;
        }

        cout << cnt << endl;
    }
    
    return 0;
}
```
