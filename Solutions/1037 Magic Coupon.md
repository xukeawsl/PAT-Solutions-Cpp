注意点：

1. 题目理解，两两相乘，总和尽可能大，不要全部都使用

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n, m;
    cin >> n;
    vector<int> coupons(n);
    for (int i = 0; i < n; i++) cin >> coupons[i];
    cin >> m;
    vector<int> values(m);
    for (int i = 0; i < m; i++) cin >> values[i];

    sort(coupons.begin(), coupons.end());
    sort(values.begin(), values.end());

    int ans = 0;
    int l = 0;

    while (l < n && l < m) {
        if (coupons[l] < 0 && values[l] < 0) {
            ans += coupons[l] * values[l];
            l++;
        } else {
            break;
        }
    }

    reverse(coupons.begin(), coupons.end());
    reverse(values.begin(), values.end());

    l = 0;
    while (l < n && l < m) {
        if (coupons[l] > 0 && values[l] > 0) {
            ans += coupons[l] * values[l];
            l++;
        } else {
            break;
        }
    }

    cout << ans;
    
    return 0;
}
```
