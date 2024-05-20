注意点：

1. 判断是否能够整除时注意分母为 0 的情况，并且考虑到数据范围，使用 long long

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    int n;
    cin >> n;
    while (n--) {
        string digit;
        cin >> digit;

        long long num = stoll(digit);
        long long a = stoll(digit.substr(0, digit.size() / 2));
        long long b = stoll(digit.substr(digit.size() / 2));

        if (a * b != 0 && num % (a * b) == 0) cout << "Yes" << endl;
        else cout << "No" << endl;
    }
    return 0;
}
```
