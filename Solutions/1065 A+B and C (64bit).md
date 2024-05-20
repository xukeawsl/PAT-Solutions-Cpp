注意点：

1. 输入的数可以被 long long 类型接受，但是两数相加的结果可能大于 long long 精度，因此可以将其转换为 `__int128_t` 类型，注意这个类型不能直接通过 cin 读取，可以通过强制类型转换转化一下
2. 如果用普通的高精度加法，需要注意一下符号的问题

```cpp
#include <iostream>

using namespace std;

int main() {
    int t;
    cin >> t;
    long long a, b, c;

    for (int i = 1; i <= t; i++) {
        cin >> a >> b >> c;
        cout << "Case #" << i << ": ";
        if ((__int128_t)a + b > c) {
            cout << "true";
        } else {
            cout << "false";
        }
        if (i != t) cout << '\n';
    }
    return 0;
}
```
