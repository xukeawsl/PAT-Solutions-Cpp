注意点：

1. 累加每个 A 的贡献即可，即对于每个 A，统计其左侧 P 的个数和右侧 T 的个数，相乘即是当前 A 对总数的贡献，注意要取模

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

int main() {
    string s;
    cin >> s;

    int n = s.size();
    
    vector<int> l(n), r(n);

    int cnt = 0;
    for (int i = 0; i < n; i++) {
        if (s[i] == 'A') {
            l[i] = cnt;
        }
        cnt += s[i] == 'P';
    }

    cnt = 0;
    for (int i = n - 1; i >= 0; i--) {
        if (s[i] == 'A') {
            r[i] = cnt;
        }
        cnt += s[i] == 'T';
    }

    
    int ans = 0, mod = 1e9 + 7;

    for (int i = 1; i < n - 1; i++) {
        if (s[i] == 'A') {
            ans = (ans + (long long)l[i] * r[i]) % mod;
        }
    }

    cout << ans << endl;
    
    return 0;
}

```
