注意点：

1. 需要用 getline 读取行
2. 使用暴力检查方法即可，用 string_view 可以避免拷贝的开销

```cpp
#include <iostream>
#include <string>
#include <string_view>

using namespace std;

bool check(string_view sv) {
    int l = 0, r = sv.size() - 1;
    while (l < r) {
        if (sv[l] != sv[r]) {
            return false;
        }
        l++;
        r--;
    }
    return true;
}

int main() {
    string str;
    getline(cin, str);

    string_view sv = str;
    int ans = 1;
    
    for (int i = 0; i < sv.size(); i++) {
        for (int len = ans + 1; len <= sv.size() - i; len++) {
            if (check(sv.substr(i, len))) {
                ans = max(ans, len);
            }
        }
    }

    cout << ans << endl;
    
    return 0;
}
```
