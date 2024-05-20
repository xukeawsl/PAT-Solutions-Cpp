注意点：题目只指明了其中一个数的进制数，不超过36进制，但是另一个数的进制数是不确定的，可能进制数非常大，取决于指定数计算出的十进制数大小，由于最多十位，最大值为 36^10 - 1，差不多是 3.6 * 10^15 次方，因此需要用 long long 来存取，long long 差不多是 10^20 次方大小，因此最大的进制数可能是 36^10 - 1 + 1，如果依次遍历的话会超时，考虑使用二分法来二分进制数，如果有多个满足条件的取最小的进制数，当找到一个满足条件的进制数时，我们可以缩短右边界尝试得到更小的进制数，二分的左边界应该是带求字符串中能表示的最小进制数，还有一个坑点是在转换为 10 进制时可能会出现 long long 越界，这时得到的结果会为负数，做二分判断时，如果得到负数，说明求得的结果太大了，这时也应该缩短右边界

```cpp
#include <iostream>
#include <string>

using namespace std;

using ll = long long;

int getdigit(char ch) {
    if (ch >= 'a' && ch <= 'z') {
        return ch - 'a' + 10;
    }

    return ch - '0';
}

ll decimal(const string& input, int base) {
    ll res = 0, mul = 1;
    for (int i = input.size() - 1; i >= 0; i--) {
        res += getdigit(input[i]) * mul;
        mul *= base;
    }
    return res;
}

bool check(const string& N1, int radix, const string& N2, int& res) {
    char ch = '\0';
    for (auto c : N2) ch = max(ch, c);

    ll a = decimal(N1, radix);
    
    ll l = getdigit(ch) + 1, r = max(a + 1, l);

    while (l < r) {
        ll base = (l + r) >> 1;
        ll dec = decimal(N2, base);
        if (dec < 0 || dec >= a) r = base;
        else l = base + 1;
    }

    ll dec = decimal(N2, l);
    res = l;
    
    return dec == a;
}

int main() {
    string N1, N2;
    int tag, radix;

    int res;

    cin >> N1 >> N2 >> tag >> radix;

    if (tag == 2) {
        swap(N1, N2);
    }

    if (check(N1, radix, N2, res)) {
        cout << res << endl;
    } else {
        cout << "Impossible" << endl;
    }
    
    return 0;
}
```
