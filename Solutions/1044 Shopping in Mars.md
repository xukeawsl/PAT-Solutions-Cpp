注意点：

1. 求区间和大于等于 m 的最小的数，然后在统计所有满足的区间，注意最后结果以区间左边界从小到大排序
2. 输出格式，最后一行末尾没有空格和回车

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>

using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    
    vector<int> sum(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> sum[i];
        sum[i] += sum[i - 1];
    }

    int lost = -1;
    for (int j = 1; j <= n; j++) {
        if (sum[j] < m) continue;
        int i = lower_bound(sum.begin(), sum.begin() + j, sum[j] - m) - sum.begin();
        if (sum[j] - sum[i] >= m) {
            if (lost == -1) lost = sum[j] - sum[i];
            lost = min(lost, sum[j] - sum[i]);
        } else {
            if (i > 0) i--;
            if (sum[j] - sum[i] >= m) {
                if (lost == -1) lost = sum[j] - sum[i];
                lost = min(lost, sum[j] - sum[i]);
            }
        }
    }

    vector<pair<int, int>> ans;
    unordered_map<int, int> pos;
    pos[0] = 0;
    for (int j = 1; j <= n; j++) {
        if (pos.count(sum[j] - lost)) {
            ans.emplace_back(pos[sum[j] - lost] + 1, j);
        }
        pos[sum[j]] = j;
    }

    sort(ans.begin(), ans.end());

    for (int i = 0; i < ans.size(); i++) {
        auto [l, r] = ans[i];
        cout << l << '-' << r;
        if (i != ans.size() - 1) cout << '\n';
    }
    
    return 0;
}
```
