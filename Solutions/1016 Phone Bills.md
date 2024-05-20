注意点：按照时间顺序来匹配两条记录，即一条 off-line 尝试和它之前的最近的 on-line 记录匹配，如果没有任何一条记录匹配就不必输出了，计算费用的时候可以暴力计算，不过要注意边界时间设置

```cpp
#include <iostream>
#include <algorithm>
#include <map>
#include <vector>
#include <cstdio>
#include <string>

using namespace std;

vector<int> rate(24);

double cacl(const string& on, const string& off, int& minu) {
    double res = 0;
    int d1, h1, m1;
    int d2, h2, m2;
    sscanf(on.c_str(), "%d:%d:%d", &d1, &h1, &m1);
    sscanf(off.c_str(), "%d:%d:%d", &d2, &h2, &m2);
    minu = (d2 * 24 * 60 + h2 * 60 + m2) - (d1 * 24 * 60 + h1 * 60 + m1);

    for (int d = d1; d <= d2; d++) {
        int sh = 0, th = 23;
        if (d == d1) sh = h1;
        if (d == d2) th = h2;
        for (int h = sh; h <= th; h++) {
            int sm = 0, tm = 60;
            if (d == d1 && h == sh) sm = m1;
            if (d == d2 && h == th) tm = m2;
            res += rate[h] * (tm - sm);
        }
    }

    return res / 100;
}

int main() {
    for (int i = 0; i < 24; i++) {
        cin >> rate[i];
    }

    map<string, vector<pair<string, int>>> record;
    map<string, string> mp;
    int n;
    cin >> n;
    
    std::string name, time, state;
    string month;
    while (n--) {
        cin >> name >> time >> state;
        month = time.substr(0, 2);
        mp[name] = month;
        if (state == "on-line") {
            record[name].emplace_back(time.substr(3), 1);
        } else {
            record[name].emplace_back(time.substr(3), 0);
        }
    }

    for (auto& [name, vec] : record) {
        sort(vec.begin(), vec.end());
        
        int p = -1;
        double total = 0;
        for (int i = 0; i < vec.size(); i++) {
            if (vec[i].second) {
                p = i;
            } else {
                if (p != -1) {
                    if (total == 0) {
                        cout << name << ' ' << mp[name] << endl;
                    }
                    int minu;
                    double price = cacl(vec[p].first, vec[i].first, minu);
                    cout << vec[p].first << ' ' << vec[i].first << ' ' << minu << ' ';
                    printf("$%.2lf\n", price);
                    total += price;
                    p = -1;
                }
            }
        }
        if (total) {
            printf("Total amount: $%.2lf\n", total);
        }
    }
    
    return 0;
}
```
