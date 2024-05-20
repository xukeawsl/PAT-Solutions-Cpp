注意点：

1. n1 和 n2 尽可能大且小于 n3，那么直接除 3 向下取整即可，剩余的分给 n3

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    string s;
    cin >> s;

    int n1 = (s.size() + 2) / 3;
    int n2 = s.size() - 2 * n1;

    for (int i = 0; i < n1 - 1; i++) {
        cout << s[i];
        for (int j = 0; j < n2; j++) cout << ' ';
        cout << s[s.size() - i - 1] << endl;
    }

    cout << s[n1 - 1];
    for (int j = 0; j < n2; j++) cout << s[n1 + j];
    cout << s[s.size() - n1] << endl;
    
    return 0;
}
```
