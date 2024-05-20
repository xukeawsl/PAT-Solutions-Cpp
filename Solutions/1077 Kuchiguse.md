注意点：

1. 空格字符也算在公共后缀中
2. 输出的字符串末尾没有多余的空格或回车

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n;
    cin >> n;
    getchar();
    
    vector<string> s(n);

    for (int i = 0; i < n; i++) {
        getline(cin, s[i]);
        reverse(s[i].begin(), s[i].end());
    }

    string ans;
    int j = 0;
    while (true) {
        char ch = s[0][j];
        bool flag = true;
        for (int i = 1; i < n; i++) {
            if (j == s[i].size()) {
                flag = false;
                break;
            }
            if (s[i][j] != ch) {
                flag = false;
            }
        }
        if (!flag) break;
        
        ans.push_back(ch);
        j++;
    }

    reverse(ans.begin(), ans.end());

    if (ans.empty()) cout << "nai";
    cout << ans;
    
    return 0;
}
```
