注意点：

1. 调整 dfs 时儿子节点的顺序不能保证得到的路径序列满足要求，因此要先得到所有路径后再排序
2. 路径需要一直遍历到叶子节点，中途满足条件不能记录路径到结果中

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int n, m, s;
vector<vector<int>> ans;
vector<int> path;
int value[101] = {0};
vector<vector<int>> tree(101);

void dfs(int u, int sum) {
    if (sum > s) return;
    if (sum == s && tree[u].empty()) {
        ans.push_back(path);
        return;
    }

    for (int v : tree[u]) {
        path.push_back(value[v]);
        dfs(v, sum + value[v]);
        path.pop_back();
    }
}

int main() {
    cin >> n >> m >> s;

    for (int i = 0; i < n; i++) {
        int val;
        cin >> val;
        value[i] = val;
    }

    for (int i = 0; i < m; i++) {
        int id, cnt;
        cin >> id >> cnt;
        vector<int> childrens(cnt);
        for (int j = 0; j < cnt; j++) {
            int c;
            cin >> c;
            childrens[j] = c;
        }
        tree[id] = move(childrens);
    }

    path.push_back(value[0]);
    dfs(0, value[0]);

    sort(ans.begin(), ans.end(), greater<vector<int>>());

    for (auto& vec : ans) {
        for (int i = 0; i < vec.size(); i++) {
            cout << vec[i];
            if (i == vec.size() - 1) cout << '\n';
            else cout << ' ';
        }
    }

    return 0;
}
```
