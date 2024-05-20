注意点：ID 是从 1 开始的，数组分配大小需要多一，思路就是层序遍历统计每一层的叶子节点数即可

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <queue>

using namespace std;

int main() {
    int n, m;
    
    cin >> n >> m;

    vector<vector<int>> childs(n + 1);
    vector<int> ans;

    int ID, K, child;
    
    for (int i = 0; i < m; i++) {
        cin >> ID >> K;
        
        while (K--) {
            cin >> child;
            childs[ID].emplace_back(child);
        }
    }

    queue<int> q;
    q.emplace(1);
    
    while (!q.empty()) {
        int s = q.size();
        int c = 0;
        for (int i = 0; i < s; i++) {
            int u = q.front(); q.pop();
            if (childs[u].empty()) c++;
            for (int v : childs[u]) {
                q.emplace(v);
            }
        }
        ans.emplace_back(c);
    }

    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i];
        if (i == ans.size() - 1) {
            cout << '\n';
        } else {
            cout << ' ';
        }
    }
    
    return 0;
}
```
