注意点：

1. 统计的是不同公共数的个数和两个集合不同数的个数

```cpp
#include <iostream>
#include <unordered_set>
#include <vector>
#include <cstdio>

using namespace std;

int main() {
    int n, m, k;

    cin >> n;

    vector<unordered_set<int>> st(n + 1);

    for (int i = 1; i <= n; i++) {
        cin >> m;
        while (m--) {
            int x;
            cin >> x;
            st[i].emplace(x);
        }
    }

    cin >> k;

    for (int i = 0; i < k; i++) {
        int q1, q2;
        cin >> q1 >> q2;
        int nc = 0, nt = 0;

        for (auto x : st[q1]) {
            if (st[q2].count(x)) {
                nc++;
            }
        }

        nt = st[q1].size() + st[q2].size() - nc;

        printf("%.1lf%%\n", nc / (nt * 0.01));
    }

    return 0;
}
```
