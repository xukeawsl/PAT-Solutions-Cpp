注意点：

1. 对于每个数，统计包含它的序列个数即可得到这个数对总和的贡献，类似中心扩展的方法，个位为左侧方案数乘以右侧方案数即 `i * (n - i + 1)`
2. 本题精度使用 `double` 无法全部通过，需要使用 `long double` 才能通过，注意 `long double` 打印时需要用 `llf` 否则打印结果为 0

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int main() {
    int n;
    cin >> n;

    long double ans = 0;

    for (int i = 1; i <= n; i++) {
        long double v;
        cin >> v;

        // 求包含当前数的序列个数为 i * (n - i + 1)
        ans += v * i * (n - i + 1);
    }

    printf("%.2llf\n", ans);

    return 0;
}
```
