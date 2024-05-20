注意点：

1. 题目所给的 ID 是 5 位数，如果以整数类型读取，则打印时格式要为 `%05d` 

```cpp
#include <iostream>
#include <cstdio>
#include <unordered_map>
#include <vector>
#include <set>

using namespace std;

int main() {
    int n, m;
    cin >> n;

    unordered_map<int, int> couple;
    
    for (int i = 0; i < n; i++) {
        int a, b;
        cin >> a >> b;
        couple[a] = b;
        couple[b] = a;
    }

    cin >> m;

    set<int> st;
    for (int i = 0; i < m; i++) {
        int x;
        cin >> x;
        if (couple.count(x) && st.count(couple[x])) {
            st.erase(couple[x]);
        } else {
            st.emplace(x);
        }
    }

    cout << st.size() << endl;

    for (auto it = st.begin(); it != st.end();) {
        printf("%05d", *it);
        ++it;
        if (it != st.end()) cout << ' ';
        else cout << '\n';
    }
    
    return 0;
}
```
