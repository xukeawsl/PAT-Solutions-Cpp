注意点：

1. 本题是 01 背包求具体方案的类型，每种面值的硬币只能至多只能用一次，因此最外层循环硬币面值，内层循环总的金钱，由于没有权重，故 dp 数组可以设置为 bool 类型，只要最终能够凑到指定的金额就算存在方案
2. 题目要求当存在多个方案时输出字典序最小的方案，并且选择硬币时不用按照给定的顺序，因此可以对硬币序列进行排序，由于递推时是从左到右的，因此我们可以将序列从大到小排序，求方案时从小到大回推即可，这样就能够保证能选小则选小

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int w[10001];
bool f[10001][101];
int ans[10001];
int idx;

int main() {
    int n, m;

    cin >> n >> m;

    for (int i = 1; i <= n; i++) {
        cin >> w[i];
    }

    sort(w + 1, w + n + 1, greater<int>());
    f[0][0] = true;
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= m; j++) {
            f[i][j] = f[i - 1][j];
            if (j - w[i] >= 0) f[i][j] = max(f[i][j], f[i - 1][j - w[i]]);
        }
    }

    if (f[n][m] == false) {
        cout << "No Solution" << endl;
        return 0;
    }

    int tot = m;
    for (int i = n; i >= 1; i--) {
        if (tot - w[i] >= 0 && f[i][tot] == f[i - 1][tot - w[i]]) {
            tot -= w[i];
            ans[idx++] = w[i];
        }
    }

    for (int i = 0; i < idx; i++) {
        cout << ans[i];
        if (i != idx - 1) cout << ' ';
        else cout << '\n';
    }
    
    return 0;
}
```
