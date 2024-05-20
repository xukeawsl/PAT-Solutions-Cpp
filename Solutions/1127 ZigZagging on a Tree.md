注意点：

1. 根据后序遍历和中序遍历序列建树，最后进行交替打印层序遍历结果

```cpp
#include <iostream>
#include <queue>
#include <algorithm>

using namespace std;

int n, idx;
int post[31], in[31];
int l[31], r[31];

int buildTree(int left, int right) {
    if (left > right) return 0;

    int u = post[idx--];

    for (int i = left; i <= right; i++) {
        if (in[i] == u) {
            r[u] = buildTree(i + 1, right);
            l[u] = buildTree(left, i - 1);
            break;
        }
    }

    return u;
}

int main() {
    cin >> n;
    idx = n - 1;
    for (int i = 0; i < n; i++) cin >> in[i];
    for (int i = 0; i < n; i++) cin >> post[i];
    int root = buildTree(0, n - 1);
    
    queue<int> q;
    q.emplace(root);

    bool flag = true;
    while (!q.empty()) {
        int m = q.size();
        vector<int> data(m);
        for (int i = 0; i < m; i++) {
            int u = q.front(); q.pop();
            data[i] = u;
            if (l[u]) q.emplace(l[u]);
            if (r[u]) q.emplace(r[u]);
        }

        if (flag) {
            reverse(data.begin(), data.end());
        }

        for (int i = 0; i < m; i++) {
            cout << data[i];
            if (i != m - 1) cout << ' ';
        }
        if (q.empty()) cout << '\n';
        else cout << ' ';
        flag = !flag;
    }
    
    return 0;
}
```
