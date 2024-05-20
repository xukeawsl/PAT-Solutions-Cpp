注意点：

1. 如果有多个满足条件的结果，尽量使较小的数最小

```cpp
#include <iostream>
#include <unordered_set>

using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    int v1 = 1e9, v2 = 1e9;
    unordered_set<int> st;
    for (int i = 0; i < n; i++) {
        int x, y;
        cin >> x;
        y = m - x;
        if (st.count(y)) {
            if (x > y) swap(x, y);
            if (x < v1) {
                v1 = x;
                v2 = y;
            }
        }
        st.insert(x);
    }

    if (v1 == 1e9) {
        cout << "No Solution" << endl;
    } else {
        cout << v1 << ' ' << v2 << endl;
    }
    
    return 0;
}
```
