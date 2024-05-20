注意点：

1. 如果存在修改的密码，需要先打印个数，因此不能边读取边打印，需要存放起来
2. 如果没有修改的密码，有两种情况，只输入了一个账户密码对，这时打印的消息和多个账户密码对的不一样需要注意

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

int main() {
    int n;
    cin >> n;
    string account, password;
    vector<pair<string, string>> ans;
    
    for (int i = 0; i < n; i++) {
        cin >> account >> password;
        bool flag = false;

        for (auto& ch : password) {
            if (ch == '1') {
                flag = true;
                ch = '@';
            } else if (ch == '0') {
                flag = true;
                ch = '%';
            } else if (ch == 'l') {
                flag = true;
                ch = 'L';
            } else if (ch == 'O') {
                flag = true;
                ch = 'o';
            }
        }
        
        if (flag) {
            ans.emplace_back(account, password);
        }
    }

    if (ans.empty()) {
        if (n == 1) {
            cout << "There is 1 account and no account is modified" << endl;
        } else {
            cout << "There are " << n << " accounts and no account is modified" << endl;
        }
    } else {
        cout << ans.size() << endl;
        for (auto& [a, p] : ans) {
            cout << a << ' ' << p << endl;
        }
    }
    return 0;
}
```
