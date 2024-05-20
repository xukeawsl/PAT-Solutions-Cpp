注意点：多项式乘法规则，题目中系数是浮点数，指数是整数，最后计算的结果需要剔除掉系数为 0 的记录，注意不要直接输出 map 的大小，需要先去掉系数为 0 的部分

```cpp
#include <iostream>
#include <map>
#include <vector>
#include <cstdio>

using namespace std;

int main() {
    map<int, double> res;

    int n, m;
    int exp;
    double coef;

    cin >> n;

    vector<pair<int, double>> a(n);
    
    for (int i = 0; i < n; i++) {
        cin >> a[i].first >> a[i].second;
    }

    cin >> m;

    for (int i = 0; i < m; i++) {
        cin >> exp >> coef;
        for (int j = 0; j < n; j++) {
            int nexp = exp + a[j].first;
            double ncoef = coef * a[j].second;
            res[nexp] += ncoef;
        }
    }

    int len = res.size();

    for (auto [_, v] : res) {
        if (v == 0) len--;
    }

    cout << len;

    for (auto iter = res.rbegin(); iter != res.rend(); iter++) {
        if (iter->second) {
            printf(" %d %.1lf", iter->first, iter->second);
        }
    }

    cout << endl;
    
    return 0;
}
```
