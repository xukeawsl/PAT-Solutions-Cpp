注意点：

1. 转 13 进制时，如果值是 0，需要自行添加一位 0

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

string getMars(int value) {
    string res;
    while (value) {
        int v = value % 13;
        if (v > 9) {
            res.push_back(v - 10 + 'A');
        } else {
            res.push_back(v + '0');
        }
        value /= 13;
    }
    if (res.size() == 0) res.push_back('0');
    if (res.size() == 1) res.push_back('0');
    reverse(res.begin(), res.end());

    return res;
}

int main() {
    int r, g, b;
    cin >> r >> g >> b;

    string ans = "#";
    ans += getMars(r);
    ans += getMars(g);
    ans += getMars(b);

    cout << ans << endl;
    
    return 0;
}
```
