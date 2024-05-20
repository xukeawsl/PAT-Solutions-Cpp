注意点：转换之后需要翻转一下数组，可以使用 <algorithm> 头文件中的 reverse 函数

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n, b;
    cin >> n >> b;

    vector<int> ans;

    while (n) {
        ans.push_back(n % b);
        n /= b;
    }

    reverse(ans.begin(), ans.end());

    bool flag = true;
    int i = 0, j = ans.size() - 1;
    while (i < j) {
        if (ans[i] != ans[j]) {
            flag = false;
            break;
        }
        i++;
        j--;
    }

    if (flag) cout << "Yes" << endl;
    else cout << "No" << endl;

    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i];
        if (i == ans.size() - 1) cout << '\n';
        else cout << ' ';
    }
    
    return 0;
}
```
