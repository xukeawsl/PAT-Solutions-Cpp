注意点：没必要转数字比较，直接拿字符串比较就行了

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    int M;

    cin >> M;

    string mint, maxt;
    string min_ID, max_ID;

    while (M--) {
        string ID, a, b;
        cin >> ID >> a >> b;
        if (mint.empty()) {
            mint = a, maxt = b;
            min_ID = max_ID = ID;
        }

        if (a < mint) {
            mint = a;
            min_ID = ID;
        }

        if (b > maxt) {
            maxt = b;
            max_ID = ID;
        }
    }

    cout << min_ID << ' ' << max_ID << endl;
    
    return 0;
}
```
