注意点：

1. 给定的节点值只说了是正数，没说范围，因此用哈希表存
2. 先根据前序和中序遍历建树，然后验证题目条件即可

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <unordered_set>
#include <unordered_map>

using namespace std;

int k, n, idx, cnt;
int pre[31], in[31];
unordered_set<int> isRed;
unordered_map<int, int> l, r;
bool flag;

int build(int left, int right) {
    if (left > right) return 0;
    int root = pre[idx++];
    for (int i = left; i <= right; i++) {
        if (in[i] == root) {
            l[root] = build(left, i - 1);
            r[root] = build(i + 1, right);
            break;
        }
    }
    if (isRed.count(root) && (isRed.count(l[root]) || isRed.count(r[root]))) {
        flag = true;
    }
    return root;
}

void dfs(int root, int black) {
    if (root == 0) {
        if (cnt == 0) cnt = black;
        else if (cnt != black) flag = true;
        return;
    }

    dfs(l[root], black + (isRed.count(l[root]) == 0));
    dfs(r[root], black + (isRed.count(r[root]) == 0));
}

int main() {
    cin >> k;
    while (k--) {
        idx = cnt = 0;
        flag = false;
        isRed.clear();
        l.clear();
        r.clear();
        cin >> n;
        for (int i = 0; i < n; i++) {
            int x;
            cin >> x;
            if (x < 0) {
                x = -x;
                isRed.emplace(x);
            }
            pre[i] = in[i] = x;
        }

        sort(in, in + n);

        int root = build(0, n - 1);
        if (isRed.count(root) || flag) {
            cout << "No" << endl;
            continue;
        }

        dfs(root, 1);
        if (flag) cout << "No" << endl;
        else cout << "Yes" << endl;
    }
    return 0;
}
```
