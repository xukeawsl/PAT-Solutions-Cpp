注意点：

1. 不用一直循环取，而是从第一个赢家的位置到最后一个人即可

```cpp
#include <iostream>
#include <cstdio>
#include <string>
#include <unordered_set>
#include <vector>

using namespace std;

int main() {
    int m, n, s;
    cin >> m >> n >> s;

    vector<string> input(m + 1);
    for (int i = 1; i <= m; i++) {
        cin >> input[i];
    }

    if (s > m) {
        cout << "Keep going..." << endl;
        return 0;
    }

    unordered_set<string> st;
    for (int i = s, cnt = n; i <= m; i++) {
        if (st.count(input[i])) continue;
        if (cnt == n) {
            cout << input[i] << endl;
            st.emplace(input[i]);
            cnt = 1;
        } else {
            cnt++;
        }
    }
    
    return 0;
}
```
