注意点：

1. 等待时间要包含提前到达的人等到 8:00:00 的部分
2. 计算平均时间的人数不算迟到的

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

int tosec(const string& t) {
    int h, m, s;
    sscanf(t.c_str(), "%d:%d:%d", &h, &m, &s);
    return h * 60 * 60 + m * 60 + s;
}

int main() {
    int n, k;
    cin >> n >> k;
    vector<int> w(k);

    vector<pair<string, int>> arrive(n);

    for (int i = 0; i < n; i++) {
        cin >> arrive[i].first >> arrive[i].second;
    }

    sort(arrive.begin(), arrive.end());

    double avg = 0;
    int count = 0;
    for (auto& [t, p] : arrive) {
        if (t >= "17:00:01") break;
        if (t < "08:00:00") {
            avg += tosec("08:00:00") - tosec(t);
            t = "08:00:00";
        }

        if (count < k) {
            w[count] = tosec(t) + p * 60;
        } else {
            int minp = 0, mins = w[0];
            for (int i = 0; i < k; i++) {
                if (w[i] < mins) {
                    mins = w[i];
                    minp = i;
                }
            }

            int curr = tosec(t);
            
            if (curr < mins) {
                avg += mins - curr;
            }

            w[minp] = max(curr, mins) + p * 60;
        }

        count++;
    }

    avg /= 60 * count;

    printf("%.1lf\n", avg);
    
    return 0;
}
```
