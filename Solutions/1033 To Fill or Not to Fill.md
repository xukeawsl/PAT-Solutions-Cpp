注意点：

1. 贪心策略：先按照位置从小到大排序，第一个必须为 0，因为初始位置在 0 且没有油，必须在第一个站加油，最后的目的地位置可以放在最后，假设一共 n 个站，则最后一个站是第 n + 1 个，我们不能一个一个站贪心的走，因为存在容量限制，以当前站为起点，从能够到达的站（由近到远）中选择第一个价格低于当前站点的，因为这样可以最小化当前站点的花费，即加油量刚好到达这个更低价的站点。如果实在选不了更低价格的，就选择尽量便宜且靠后的站点，这样用当前站点的价格加满油，能够得到更便宜的总价，如果当前站点加满油都无法再到达下一个站点，则能到达的最大距离就是当前位置+满油能够走的距离
2. 这个最大距离是浮点数，因此加的油并非整数而是浮点数，不是用向上取整的方式

```cpp
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>

using namespace std;

int c, d, avg, n;

int main() {
    cin >> c >> d >> avg >> n;

    vector<pair<double, double>> station;
    
    for (int i = 0; i < n; i++) {
        double price, pos;
        cin >> price >> pos;
        station.emplace_back(pos, price);
    }

    station.emplace_back(d, 0);

    sort(station.begin(), station.end());

    if (station[0].first) {
        printf("The maximum travel distance = %.2lf\n", 0.0);
        return 0;
    }

    double curr = 0;
    double max_dist = 0;
    double total = 0;

    int i;
    for (i = 0; i < n;) {
        int k = -1;
        for (int j = i + 1; j <= n; j++) {
            int dist = station[j].first - station[i].first;
            if (dist > c * avg) break;
            if (station[j].second < station[i].second) {
                k = j;
                break;
            } else if (k == -1 || station[j].second < station[k].second) {
                k = j;
            }
        }
        if (k == -1) {
            max_dist = station[i].first + c * avg;
            break;
        }
        if (station[k].second <= station[i].second) {
            double need = (station[k].first - station[i].first) / avg;
            if (need >= curr) {
                need -= curr;
                curr = 0;
                total += need * station[i].second;
            } else {
                curr -= need;
            }
        } else {
            double need = (station[k].first - station[i].first) / avg;
            double add = c - curr;
            curr = c - need;
            total += add * station[i].second;
        }
        i = k;
    }

    if (i < n) {
        printf("The maximum travel distance = %.2lf\n", max_dist);
    } else {
        printf("%.2lf\n", total);
    }
    
    return 0;
}
```
