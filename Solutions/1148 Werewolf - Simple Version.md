注意点：

1. 根据题目含义，恰好有一个狼和一个人说谎，因此遍历两个狼的所有情况，然后验证说谎的分布情况，如果刚好一个狼和一个人说谎，则找到一组答案

```cpp
#include <iostream>

using namespace std;

int a[101];

bool islying(int i, int j, int x) {
    int t = abs(a[x]);
    if (a[x] < 0) {
        if (t != i && t != j) {
            return true;
        }
    } else {
        if (t == i || t == j) {
            return true;
        }
    }
    return false;
}

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) cin >> a[i];

    for (int i = 1; i <= n; i++) {
        for (int j = i + 1; j <= n; j++) {
            int lying_w = 0, lying_p = 0;
            for (int k = 1; k <= n; k++) {
                if (islying(i, j, k)) {
                    if (k == i || k == j) lying_w++;
                    else lying_p++;
                }
            }
            if (lying_w == 1 && lying_p == 1) {
                cout << i << ' ' << j << endl;
                return 0;
            }
        }
    }

    cout << "No Solution" << endl;
    return 0;
}
```
