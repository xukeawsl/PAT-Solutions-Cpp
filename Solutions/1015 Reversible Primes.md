注意点：当给出的数以及转为 D 进制翻转后的数都是素数时才算满足条件

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

bool isprime(int n) {
    if (n < 2) return false;

    for (int i = 2; i <= sqrt(n); i++) {
        if (n % i == 0) {
            return false;
        }
    }

    return true;
}

int main() {
    int n, d;

    while (true) {
        cin >> n;
        if (n < 0) break;
        cin >> d;

        vector<int> radix;

        int k = n;
        while (k) {
            radix.push_back(k % d);
            k /= d;
        }

        reverse(radix.begin(), radix.end());

        int mul = 1;
        int x = 0;
        for (int e : radix) {
            x += e * mul;
            mul *= d;
        }

        if (isprime(n) && isprime(x)) {
            cout << "Yes" << endl;
        } else {
            cout << "No" << endl;
        }
    }
    
    return 0;
}
```
