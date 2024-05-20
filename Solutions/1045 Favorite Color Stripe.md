注意点：

1. 二维 dp ，第一维是给定的序列的下标，第二位是喜欢颜色的序列中的下标，如果当前序列下标对应的颜色是喜欢的，则可以将当前颜色计算在内，此时 f[i][j] = f[i - 1][j] + 1，当然也可以从上个喜欢的颜色序列的状态转移过来，即 f[i - 1][j - 1] + 1，取大的值，如果不是则正常从上一次转移，同样也可以从上个喜欢的颜色序列转移过来，不过不需要 +1

```cpp
#include <iostream>
#include <vector>

using namespace std;


int main() {
    int n, m, l;

    cin >> n;

    cin >> m;
    vector<int> favorite(m);
    for (int i = 0; i < m; i++) cin >> favorite[i];

    cin >> l;
    vector<int> vec(l);
    for (int i = 0; i < l; i++) cin >> vec[i];

    vector<vector<int>> f(l, vector<int>(m));

    int ans = 0;
    
    for (int i = 0; i < l; i++) {
        for (int j = 0; j < m; j++) {
            if (vec[i] == favorite[j]) {
                if (i > 0) {
                    f[i][j] = f[i - 1][j] + 1;
                    if (j > 0) f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);
                }
                else f[i][j] = 1;
            } else {
                if (i > 0) f[i][j] = f[i - 1][j];
                if (j > 0) f[i][j] = max(f[i][j], f[i][j - 1]);
            }
            ans = max(ans, f[i][j]);
        }
    }

    cout << ans << endl;

    return 0;
}
```
