注意点：理解题意，电梯最初在 0 层，上升一层 6s，下降一层 4s，每到指定层数停留 5s

```cpp
#include <iostream>

using namespace std;

int main() {
    int N;
    cin >> N;

    int a;

    int ans = N * 5;
    int pre = 0;
    
    for (int i = 0; i < N; i++) {
        cin >> a;

        if (a > pre) {
            ans += (a - pre) * 6;
        } else {
            ans += (pre - a) * 4;
        }

        pre = a;
    }

    cout << ans << endl;
    
    return 0;
}
```
