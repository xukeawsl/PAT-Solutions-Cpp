注意点：

1. 行尾没有多余空格和回车

```cpp
#include <iostream>
#include <set>

using namespace std;

int main() {
    int n;
    cin >> n;
    set<int> st;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        int digit = 0;
        while (x) {
            digit += x % 10;
            x /= 10;
        }
        st.emplace(digit);
    }

    cout << st.size() << endl;
    for (auto it = st.begin(); it != st.end(); ) {
        cout << *it;
        ++it;
        if (it != st.end()) cout << ' ';
    }
    return 0;
}
```
