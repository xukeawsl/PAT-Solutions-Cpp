注意点：

1. 对于全 0 的输入来说，它的指数 k 为 0，保留位都是 0，位数根据题目要求即可
2. 计算指数位 k 的值，可以通过计算第一个有效位的下标减去小数点下标的值得到，前提是含有一个有效位，否则对于全 0 来说 k 为 0
3. 如果去除前导 0 后剩余的长度大于等于需要保留的位数，则截取，否则填充若干个 0，可以使用 `string(n, '0')`来拼接 n 个 0

```cpp
#include <iostream>
#include <string>

using namespace std;

string get(string s, int n) {
    int k = s.find('.');
    if (k == -1) k = s.size();
    else s = s.substr(0, k) + s.substr(k + 1);
    int idx = 0;
    while (idx < s.size() && s[idx] == '0') idx++;

    if (idx == s.size()) {
        k = 0;
    } else {
        k = k - idx;
    }
    
    s = s.substr(idx);
    
    if (n <= s.size()) {
        s = s.substr(0, n);
    } else {
        s += string(n - s.size(), '0');
    }

    return "0." + s + "*10^" + to_string(k);
}

int main() {
    int n;
    string a, b;

    cin >> n >> a >> b;

    string res1 = get(a, n);
    string res2 = get(b, n);

    if (res1 == res2) {
        cout << "YES " << res1 << endl;
    } else {
        cout << "NO " << res1 << ' ' << res2 << endl;
    }

    return 0;
}
```
