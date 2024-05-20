注意点：

1. 分式化简，以及负数打印的格式
2. 题目给到的数据类型需要用 long int
3. 当存在有理分数整数时，在打印分式前要加空格，如果分子为 0，直接打印一个 0

```cpp
#include <iostream>
#include <string>
#include <numeric>

using namespace std;

void print(long int au, long int ad) {
    if (au < 0 && ad < 0) {
        print(-au, -ad);
        return;
    }

    if (ad == 0) {
        cout << "Inf";
        return;
    }

    if (au == 0) {
        cout << 0;
        return;
    }
    
    if (au < 0 || ad < 0) cout << "(-";
    long int u = abs(au);
    long int d = abs(ad);
    long int cnt = u / d;
    u -= cnt * d;

    long int g = gcd(u, d);
    u /= g;
    d /= g;

    if (cnt > 0) cout << cnt;
    if (u > 0) {
        if (cnt > 0) cout << ' ';
        cout << u << '/' << d;
    }

    if (au < 0 || ad < 0) cout << ")";
}

int main() {
    string a, b;
    cin >> a >> b;

    int k = a.find('/');
    long int au = stol(a.substr(0, k));
    long int ad = stol(a.substr(k + 1));

    k = b.find('/');
    long int bu = stol(b.substr(0, k));
    long int bd = stol(b.substr(k + 1));

    print(au, ad);
    cout << " + ";
    print(bu, bd);
    cout << " = ";
    print(au * bd + bu * ad, ad * bd);
    cout << endl;


    print(au, ad);
    cout << " - ";
    print(bu, bd);
    cout << " = ";
    print(au * bd - bu * ad, ad * bd);
    cout << endl;

    print(au, ad);
    cout << " * ";
    print(bu, bd);
    cout << " = ";
    print(au * bu, ad * bd);
    cout << endl;

    print(au, ad);
    cout << " / ";
    print(bu, bd);
    cout << " = ";
    print(au * bd, ad * bu);
    cout << endl;

    return 0;
}
```
