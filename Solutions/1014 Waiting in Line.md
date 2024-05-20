注意点：n 个窗口，每个窗口 m 个可等待位置，因此前 n * m 个顾客依次排着就行了，后面每新进入一个人就淘汰一个最早完成业务的，最后没有新进入的顾客后把计算一下排队中顾客的处理时间（坑点：窗口可以在 17:00 之前办理业务，但是不能根据结束时间来判断是否输出 Sorry，应该是开始处理时的起始时间是否大于等于 17:00，最后的输出结果是可能超过 17:00 的，因为业务办理时间可能比较长，这种情况是允许的）

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <cstdio>

using namespace std;

int main() {
    int n, m, k, q, t, c;
    cin >> n >> m >> k >> q;

    vector<queue<pair<int, int>>> window(n);
    vector<int> base(n);
    vector<int> time(k + 1, -1);

    int p = 0;
    int total = 0;
    for (int i = 0; i < k; i++) {
        cin >> t;
        if (total < n * m) {
            window[p].emplace(t, i + 1);
            p = (p + 1) % n;
            total++;
        } else {
            int mint = 1e9;
            int minp = -1;
            for (p = 0; p < n; p++) {
                if (base[p] >= 9 * 60) continue;
                if (base[p] + window[p].front().first < mint) {
                    mint = base[p] + window[p].front().first;
                    minp = p;
                }
            }
            if (minp == -1) continue;
            auto [d, c] = window[minp].front();
            base[minp] += d;
            time[c] = base[minp];
            window[minp].pop();
            window[minp].emplace(t, i + 1);
        }
    }

    for (int i = 0; i < n; i++) {
        auto& q = window[i];
        while (!q.empty()) {
            if (base[i] >= 9 * 60) break;
            auto [d, c] = q.front(); q.pop();
            base[i] += d;
            time[c] = base[i];
        }
    }

    for (int i = 0; i < q; i++) {
        cin >> c;
        int m = time[c];
        int h = m / 60;
        m %= 60;
        h += 8;
        if (time[c] == -1) {
            cout << "Sorry" << endl;
        } else {
            printf("%02d:%02d\n", h, m);
        }
    }
    
    return 0;
}
```
