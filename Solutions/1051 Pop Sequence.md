注意点：

1. 限制了栈大小的出栈序列检查

```cpp
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

int main() {
    int n, m, k;
    cin >> n >> m >> k;

    vector<int> seq(m);
        
    for (int i = 0; i < k; i++) {
        for (int j = 0; j < m; j++) {
            cin >> seq[j];
        }

        stack<int> stk;
        bool flag = true;
        for (int j = 0, v = 1; j < m;) {
            if (stk.size() > n) {
                flag = false;
                break;
            }
            
            if (!stk.empty() && stk.top() == seq[j]) {
                stk.pop();
                j++;
            } else {
                stk.push(v++);
            }
        }

        if (flag) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
    
    return 0;
}
```
