注意点：

1. 输出第一个总共出现一次的数，没有则输出 None

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>

using namespace std;

int main() {
    int n;
    cin >> n;
    unordered_map<int, int> cnt;
    vector<int> input(n);
    for (int i = 0; i < n; i++) {
        cin >> input[i];
        cnt[input[i]]++;
    }

    int ans = -1;
    for (int i = 0; i < n; i++) {
        if (cnt[input[i]] == 1) {
            ans = input[i];
            break;
        }
    }

    if (ans == -1) cout << "None" << endl;
    else cout << ans << endl;
    
    return 0;
}
```
