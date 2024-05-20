注意点：

1. 理解题目含义再模拟，从左到右依次对相同的字符计数

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    string s;
    cin >> s;
    int n;
    cin >> n;

    for (int i = 1; i < n; i++) {
        int cnt = 1, ch = s[0];
        string ns;
        for (int j = 1; j < s.size(); j++) {
            if (s[j] != ch) {
                ns.push_back(ch);
                ns += to_string(cnt);
                cnt = 1;
                ch = s[j];
            } else {
                cnt++;
            }
        }
        ns.push_back(ch);
        ns += to_string(cnt);
        s = ns;
    }

    cout << s << endl;
    
    return 0;
}
```
