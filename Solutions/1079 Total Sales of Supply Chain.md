注意点：

1. 根据题目要求建立多叉树，然后遍历到叶子节点，中间每过一层就要更新一下单价，最后到叶子节点用数量乘以单价

```cpp
#include <iostream>
#include <unordered_map>
#include <cstdio>
#include <vector>

using namespace std;

int n;
double p, r, ans;
unordered_map<int, int> amount;
unordered_map<int, vector<int>> mp;

void dfs(int u, double price) {
    if (amount.count(u)) {
        ans += amount[u] * price;
        return;
    }

    for (int v : mp[u]) {
        dfs(v, price + price * 0.01 * r);
    }
}

int main() {
    cin >> n >> p >> r;

    for (int i = 0; i < n; i++) {
        int cnt, x;
        cin >> cnt;
        if (cnt == 0) {
            cin >> x;
            amount[i] = x;
        } else {
            for (int j = 0; j < cnt; j++) {
                cin >> x;
                mp[i].emplace_back(x);
            }
        }
    }

    dfs(0, p);

    printf("%.1lf\n", ans);
    
    return 0;
}
```
