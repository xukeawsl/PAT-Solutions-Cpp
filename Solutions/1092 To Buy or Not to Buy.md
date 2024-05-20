注意点：

1. 计数统计即可

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

using namespace std;

int main() {
    string a, b;
    cin >> a >> b;
    int ans = 0;
    unordered_map<char, int> cnt;
    for (auto ch : a) cnt[ch]++;

    bool flag = true;

    for (auto ch : b) {
        if (cnt[ch] == 0) {
            flag = false;
            ans++;
        } else {
            cnt[ch]--;
        }
    }

    if (flag) {
        cout << "Yes " << a.size() - b.size() << endl;
    } else {
        cout << "No " << ans << endl;
    }
    
    return 0;
}
```
