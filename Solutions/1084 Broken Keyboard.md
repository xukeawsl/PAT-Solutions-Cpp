注意点：

1. 按键包括数字、字母（大小写不敏感）和 _（空格）
2. 输出顺序按照检测顺序，且不能重复

```cpp
#include <iostream>
#include <unordered_set>
#include <string>

using namespace std;

int main() {
    string org, type;
    cin >> org;
    cin >> type;

    unordered_set<char> st;

    for (char ch : type) {
        st.emplace(toupper(ch));
    }

    string ans;
    unordered_set<char> out;
    
    for (char ch : org) {
        ch = toupper(ch);
        if (!st.count(ch) && !out.count(ch)) {
            out.emplace(ch);
            ans += ch;
        }
    }
    
    cout << ans << endl;
    
    return 0;
}
```
