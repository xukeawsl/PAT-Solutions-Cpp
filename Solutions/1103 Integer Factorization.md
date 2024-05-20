注意点：

1. 在 PAT 使用暴力回溯+剪枝是可以过的，注意枚举的时候从大到小
2. 正解应该是完全背包 dp 算法，下面两种做法都贴一下

**解法一：dfs + 剪枝**

```cpp
#include <iostream>
#include <vector>
#include <cmath>
#include <numeric>

using namespace std;

long long f[401];
int n, k, p;
vector<int> ans, path;
int smax;

void dfs(int sum, int pre, int len, int s) {
    if (sum > n) return;
    if (len > k) return;
    if (sum == n && len == k) {
        if (s > smax) {
            ans = path;
            smax = s;
            return;
        }
    }

    for (int t = pre; t >= 1; t--) {
        if (sum + f[t] > n) continue;
        path.emplace_back(t);
        dfs(sum + f[t], t, len + 1, s + t);
        path.pop_back();
    }
}

int main() {
    cin >> n >> k >> p;

    int start = 1;
    for (int i = 1; i <= n; i++) {
        f[i] = pow(i, p);
        if (f[i] <= n) {
            start = i;
        }
    }

    dfs(0, start, 0, 0);
    
    if (!ans.empty()) {
        cout << n << " = ";
        for (int i = 0; i < ans.size(); i++) {
            cout << ans[i] << "^" << p;
            if (i != ans.size() - 1) cout << " + ";
            else cout << '\n';
        }
    } else {
        cout << "Impossible" << endl;
    }
    
    return 0;
}
```

**解法二：完全背包求具体方案**

完全背包指的是每个物品都可以选若干次，对于本题来说就是选若干个数，每个数就相当于物品，它对应的体积是数的 p 次方，那么只要这个数的 p 次方不超过题目要求的总和（总体积）即可，对于本题来说，p 最小为 2 体积最大为 400，则因子最大为 20，然后就是题目要求选择的物品数量限制为 k 个，因此需要加一维来限制长度，用状态 `f[i][j][k]` 来表示从 1 ~ i 的物品中选若干个，体积为 j 且数量为 k 时，能够得到的最大**因子和**
默认状态是无穷小，初始状态为 `f[0][0][0]` ，这时最大因子和为 0，对于当前物品，当不选择时，其状态 `f[i][j][k] = f[i - 1][j][k]` ，当 `j >= i^p 且 k > 0` 时，可以考虑取当前物品，这时 `f[i][j][k] = max(f[i][j][k], f[i][j - i^p][k - 1] + i)` ，这里需要注意，由于同一个物品可以取多次，因此这里不是 `f[i - 1][j - i^p][k - 1]` 而是 `f[i][j - i^p][k - 1]`
当因子和相同时，优先选择更大的物品使得因子序列尽可能的大，因此在求具体方案时，优先从大因子取，能取大因子就取大因子

```cpp
#include <iostream>
#include <cmath>
#include <cstring>

using namespace std;

int f[21][401][401];

int main() {
    int n, m, p;
    
    cin >> n >> m >> p;
    
    memset(f, 0xcf, sizeof(f));
    f[0][0][0] = 0;

    int i;
    for (i = 1; ; i++) { // 从 1 ~ i 这些物品中选
        int v = pow(i, p);  // 物品 i 对应的体积
        if (v > n) break;
        int w = i;
        for (int j = 0; j <= n; j++) {
            for (int k = 0; k <= m; k++) {
                f[i][j][k] = f[i - 1][j][k];
                if (j >= v && k > 0) f[i][j][k] = max(f[i][j][k], f[i][j - v][k - 1] + w);
            }
        }
    }
    
    int t = i - 1;  // 能放的最大的物品
    int tot = f[t][n][m];
    
    if (tot < 0) {
        cout << "Impossible" << endl;
        return 0;
    }
    
    cout << n << " = ";
    
    for (int i = t, j = n, k = m; i >= 1; i--) {
        int v = pow(i, p);
        while (f[t][j - v][k - 1] + i == tot) {
            j -= v;
            k -= 1;
            tot -= i;
            
            cout << i << "^" << p;
            if (tot == 0) cout << '\n';
            else cout << " + ";
        }
    }
    
    return 0;
}
```
