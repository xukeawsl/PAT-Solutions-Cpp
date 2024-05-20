注意点：

1. 月饼的需求量可能是浮点数，示例中看着是整数，有点坑
2. 贪心地取单价最低的方案，可以通过排序也可以使用堆，需要自定义一下排序规则

```cpp
#include <iostream>
#include <queue>
#include <cstdio>

using namespace std;

using PDD = pair<double, double>;

double v[1001], w[1001];

int main() {
    int n;
    double d;

    cin >> n >> d;

    auto cmp = [](const PDD& p1, const PDD& p2) {
        return p1.first * p2.second > p2.first * p1.second;
    };

    priority_queue<PDD, vector<PDD>, decltype(cmp)> pq(cmp);

    for (int i = 0; i < n; i++) cin >> v[i];
    for (int i = 0; i < n; i++) {
        cin >> w[i];
        pq.emplace(v[i], w[i]);
    }

    double ans = 0;

    while (!pq.empty()) {
        auto p = pq.top(); pq.pop();
        if (p.first <= d) {
            d -= p.first;
            ans += p.second;
        } else {
            ans += p.second * d / p.first;
            break;
        }
    }

    printf("%.2lf\n", ans);

    return 0;
}
```
