注意点：

1. 理解题意，检查图中的所有边是否至少与给定的点集中的一个相连

```cpp
#include <iostream>
#include <unordered_set>

using namespace std;

int e[10000][2];

int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        cin >> e[i][0] >> e[i][1];
    }

    int k;
    cin >> k;

    for (int i = 0; i < k; i++) {
        unordered_set<int> st;
        int nv;
        cin >> nv;
        for (int j = 0; j < nv; j++) {
            int v;
            cin >> v;
            st.emplace(v);
        }

        bool flag = true;
        for (int j = 0; j < m; j++) {
            if (!st.count(e[j][0]) && !st.count(e[j][1])) {
                flag = false;
                break;
            }
        }
        if (flag) cout << "Yes" << endl;
        else cout << "No" << endl;
    }
    
    return 0;
}
```
