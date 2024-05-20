注意点：

1. 有两种选择，可以正着走也可以倒着走，可以使用前缀和，注意题目给的两个点不一定是 x < y 的，可以调整一下

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int n, m;
    cin >> n;
    vector<int> sum(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> sum[i];
        sum[i] += sum[i - 1];
    }

    cin >> m;
    for (int i = 0; i < m; i++) {
        int x, y;
        cin >> x >> y;
        if (x > y) swap(x, y);
        cout << min(sum[x - 1] + sum[n] - sum[y - 1], sum[y - 1] - sum[x - 1]) << endl;
    }

    return 0;
}
```
