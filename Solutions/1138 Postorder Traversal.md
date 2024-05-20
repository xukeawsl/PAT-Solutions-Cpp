注意点：

1. 根据前序和中序遍历建树后进行后序遍历，记录第一个遍历的节点即可

```cpp
#include <iostream>
#include <unordered_map>

using namespace std;

int pre[50001], in[50001];
int n, idx, ans = -1;

unordered_map<int, int> pos;
unordered_map<int, int> l, r;

int build(int left, int right) {
    if (left > right) return -1;
    int root = pre[idx++];
    int p = pos[root];
    l[root] = build(left, p - 1);
    r[root] = build(p + 1, right);
    return root;
}

void dfs(int root) {
    if (!root) return;
    if (ans != -1) return;

    dfs(l[root]);
    dfs(r[root]);

    if (ans == -1) ans = root;
}

int main() {
    cin >> n;

    for (int i = 0; i < n; i++) cin >> pre[i];
    for (int i = 0; i < n; i++) {
        cin >> in[i];
        pos[in[i]] = i;
    }

    int root = build(0, n - 1);

    dfs(root);

    cout << ans << endl;
    
    return 0;
}
```
