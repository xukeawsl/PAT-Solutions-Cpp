注意点：

1. 字符串按行读取

```cpp
#include <iostream>
#include <unordered_set>

using namespace std;

int main() {
    string s1, s2;
    string s;
    unordered_set<char> st;
    
    getline(cin, s1);
    getline(cin, s2);

    for (char ch : s2) st.insert(ch);

    for (char ch : s1) {
        if (st.count(ch)) continue;
        s.push_back(ch);
    }

    cout << s << endl;
    
    return 0;
}
```
