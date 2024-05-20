注意点：

1. 最小公倍数可能超过 `long int` ，可以使用 `__int128_t` 类型来存放
2. 思路是先求所有分母的最小公倍数，然后对分子依次进行扩大，相加后对分子分母进行约分，即除以最大公约数，然后将整数倍提取出来

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

__int128_t lcm(__int128_t a, __int128_t b) {
    return a * b / __gcd(a, b);
}

int main() {
    int n;

    cin >> n;

    vector<pair<__int128_t, __int128_t>> vec;

    __int128_t x = 0;
    __int128_t y = 1;
    
    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;
        int k = s.find('/');
        __int128_t a = (__int128_t)stol(s.substr(0, k));
        __int128_t b = (__int128_t)stol(s.substr(k + 1));

        vec.emplace_back(a, b);
        y = lcm(y, b);
    }

    for (int i = 0; i < n; i++) {
        x += vec[i].first * (y / vec[i].second);
    }

    __int128_t common = __gcd(x, y);
    x /= common;
    y /= common;

    if (y == 1) {
        cout << (long int)x << endl;
    } else {
        common = x / y;
        if (common > 0) {
            x -= common * y;
            cout << (long int)common << ' ';
        }
        cout << to_string((long int)x) + "/" + to_string((long int)y) << endl;
    }

    return 0;
}
```
