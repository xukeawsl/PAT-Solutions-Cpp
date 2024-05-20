注意点：

1. 题目需要查询某个时刻停车场中的车辆数，因此可以使用差分数组的性质实现区间加法，最后求前缀和后即可直接查询指定时刻的停车数量
2. 差分数组的最大大小不会超过 24 * 60 * 60，因此 O(n) 复杂度是可以的
3. 给定的时间是乱序的，因此需要按时间排序

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <unordered_map>
#include <cstdio>

using namespace std;

const int N = 24 * 60 * 60 + 1;

int d[N];

struct Car {
    string plate;
    int time;
    bool out;
};

int toInt(const string& st) {
    int h, m, s;
    sscanf(st.c_str(), "%d:%d:%d", &h, &m, &s);
    return h * 60 * 60 + m * 60 + s;
}

string toStr(int total) {
    int h = total / 3600;
    int m = (total - h * 3600) / 60;
    int s = total % 60;

    char ret[30];
    sprintf(ret, "%02d:%02d:%02d", h, m, s);

    return ret;
}

int main() {
    int n, k;
    cin >> n >> k;

    vector<Car> cars(n);
    for (int i = 0; i < n; i++) {
        string t, act;
        cin >> cars[i].plate >> t >> act;
        cars[i].time = toInt(t);
        cars[i].out = act == "out";
    }

    sort(cars.begin(), cars.end(), [](const Car& c1, const Car& c2) {
        return c1.time < c2.time;
    });

    
    unordered_map<string, int> intime;
    unordered_map<string, int> total;

    int maxt = 0;
    
    for (int i = 0; i < n; i++) {
        if (cars[i].out) {
            if (!intime.count(cars[i].plate) || intime[cars[i].plate] == -1) {
                continue;
            }

            int in = intime[cars[i].plate];
            int out = cars[i].time;

            d[in]  += 1;
            d[out] -= 1;

            total[cars[i].plate] += out - in;
            maxt = max(maxt, total[cars[i].plate]);

            intime[cars[i].plate] = -1;
            
        } else {
            intime[cars[i].plate] = cars[i].time;
        }
    }

    for (int i = 1; i < N; i++) {
        d[i] += d[i - 1];
    }
    
    for (int i = 0; i < k; i++) {
        string t;
        cin >> t;
        int tick = toInt(t);
        cout << d[tick] << endl;
    }

    vector<string> ans;
    for (auto& [plate, t] : total) {
        if (t == maxt) {
            ans.emplace_back(plate);
        }
    }
    sort(ans.begin(), ans.end());

    for (auto& s : ans) cout << s << ' ';
    cout << toStr(maxt) << endl;
    
    return 0;
}
```
