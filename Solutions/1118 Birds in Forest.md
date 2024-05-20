注意点：

1. 照片和鸟的编号不一样，通过将鸟的编号加上一个偏移来使得编号唯一
2. 通过并查集进行合并，属于同一个集合的鸟属于同一颗树

```cpp
#include <iostream>
#include <vector>
#include <numeric>
#include <algorithm>

using namespace std;

const int DIF = 10000;

vector<int> ut(20001);

int Find(int x) {
    if (ut[x] == x) return x;
    return ut[x] = Find(ut[x]);
}

void Union(int x, int y) {
    int rootx = Find(x);
    int rooty = Find(y);

    if (rootx == rooty) return;

    ut[rootx] = rooty;
}

int main() {
    iota(ut.begin(), ut.end(), 0);

    int n, m = 0;
    cin >> n;

    for (int i = 0; i < n; i++) {
        int k, b;
        cin >> k;
        for (int j = 0; j < k; j++) {
            cin >> b;
            m = max(m, b);
            Union(b + DIF, i);
        }
    }

    int cnt = 0;
    for (int i = 0; i < n; i++) {
        if (ut[i] == i) cnt++;
    }

    cout << cnt << ' ' << m << endl;

    int q;
    cin >> q;

    for (int i = 0; i < q; i++) {
        int x, y;
        cin >> x >> y;
        if (Find(x + DIF) == Find(y + DIF)) {
            cout << "Yes" << endl;
        } else {
            cout << "No" << endl;
        }
    }
    
    return 0;
}
```
