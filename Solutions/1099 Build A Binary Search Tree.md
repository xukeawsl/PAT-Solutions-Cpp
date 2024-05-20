注意点：

1. 利用二叉搜索树中序遍历结果有序的性质建树即可

```cpp
#include <iostream>
#include <cstring>
#include <queue>
#include <algorithm>

using namespace std;

int n, idx;
int a[101], val[101];
int l[101], r[101];

void dfs(int root) {
    if (root == -1) return;
    dfs(l[root]);
    val[root] = a[idx++];
    dfs(r[root]);
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> l[i] >> r[i];
    }

    for (int i = 0; i < n; i++) cin >> a[i];

    sort(a, a + n);

    dfs(0);

    queue<int> q;
    q.emplace(0);

    while (!q.empty()) {
        int s = q.size();
        for (int i = 0; i < s; i++) {
            int u = q.front(); q.pop();
            if (l[u] != -1) q.emplace(l[u]);
            if (r[u] != -1) q.emplace(r[u]);

            cout << val[u];
            if (q.empty()) cout << '\n';
            else cout << ' ';
        }
    }

    return 0;
}
```
