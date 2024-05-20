注意点：

1. 大于 1 的正整数都可以进行质因数分解，题目给出正整数，因此需要对 1 进行特判
2. 分解质因数的时间复杂度是根号 n 的，注意如果最后 t 不为 1，则 t 本身是最后一个质因数

```cpp
#include <iostream>
#include <vector>
#include <cmath>

using namespace std;

int main() {
    long int N;
    cin >> N;

    if (N == 1) {
        cout << "1=1" << endl;
        return 0;
    }

    vector<pair<int, int>> factor;

    long int t = N;
    for (long int i = 2; i <= sqrt(t); i++) {
        if (t % i) continue;
        int cnt = 0;
        while (t % i == 0) {
            t /= i;
            cnt++;
        }
        factor.emplace_back(i, cnt);
    }

    if (t > 1) {
        factor.emplace_back(t, 1);
    }

    cout << N;
    bool first = true;
    for (auto [p, k] : factor) {
        if (first) {
            cout << "=";
            first = false;
        } else {
            cout << "*";
        }

        if (k == 1) {
            cout << p;
        } else {
            cout << p << '^' << k;
        }
    }
    cout << endl;
    
    return 0;
}
```
