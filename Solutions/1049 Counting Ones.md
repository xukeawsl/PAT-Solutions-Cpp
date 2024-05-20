注意点：

1. 数据量很大，不能采用暴力做法，统计每个数位为 1 时的贡献，需要考虑当前数位等于 1 或大于 1 的情况
2. 使用的 mask 可能会很大，需要用 long long 来存放

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n;
    cin >> n;

    long long left = 0, right = n, mask = 1;

    vector<int> digit;
    while (n) {
        digit.push_back(n % 10);
        n /= 10;
        mask *= 10;
    }

    reverse(digit.begin(), digit.end());

    int ans = 0;
    for (int i = 0; i < digit.size(); i++) {
        mask /= 10;
        ans += left * mask;
        right %= mask;
        if (digit[i] == 1) {
            ans += 1 + right;
        } else if (digit[i] > 1) {
            ans += mask;
        }
        left = left * 10 + digit[i];
    }

    cout << ans << endl;
    
    return 0;
}
```
