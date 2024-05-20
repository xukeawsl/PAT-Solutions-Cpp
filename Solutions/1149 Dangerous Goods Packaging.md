注意点：

1. 需要做一点时间复杂度优化，不能一起的物品对数只有 100，但是每组最多输入 1000，因此可以过滤掉不存在对应关系的物品，这样时间复杂度就能降低到 O(M^3)

```cpp
#include <iostream>
#include <unordered_map>
#include <unordered_set>
#include <vector>

using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    unordered_map<int, unordered_set<int>> mp;
    
    for (int i = 0; i < n; i++) {
        int a, b;
        cin >> a >> b;
        mp[a].emplace(b);
        mp[b].emplace(a);
    }

    for (int i = 0; i < m; i++) {
        int k;
        cin >> k;
        vector<int> G;
        for (int j = 0; j < k; j++) {
            int x;
            cin >> x;
            if (mp.count(x)) {
                G.emplace_back(x);
            }
        }

        k = G.size();
        bool flag = true;
        for (int j = 0; j < k; j++) {
            for (int t = j + 1; t < k; t++) {
                if (mp[G[j]].count(G[t])) {
                    flag = false;
                    break;
                }
            }
            if (!flag) break;
        }

        if (flag) cout << "Yes" << endl;
        else cout << "No" << endl;
    }
    
    return 0;
}
```
