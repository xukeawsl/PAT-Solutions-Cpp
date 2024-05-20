注意点：

1. 输入的数最大有 20 位，但是 unsinged long long 只有 1e20 的样子，需要使用高精度乘法

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

int main() {
    string n;
    cin >> n;

    int cnt[10] = {0};

    vector<int> res;
    int digit = 0;
    for (int i = n.size() - 1; i >= 0; i--) {
        int x = n[i] - '0';
        cnt[x]++;
        digit += x * 2;
        res.push_back(digit % 10);
        digit /= 10;
    }

    while (digit) {
        res.push_back(digit % 10);
        digit /= 10;
    }

    reverse(res.begin(), res.end());
    
    for (int x : res) cnt[x]--;

    bool flag = true;
    for (int i = 0; i < 10; i++) {
        if (cnt[i]) {
            flag = false;
            break;
        }
    }

    if (flag) cout << "Yes" << endl;
    else cout << "No" << endl;

    for (int x : res) cout << x;
    cout << endl;
    
    return 0;
}
```
