注意点：

1. 排序后从左至右两两链接即可

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int seg[10001];

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; i++) {
        cin >> seg[i];
    }

    sort(seg, seg + n);

    int cur = seg[0];
    for (int i = 1; i < n; i++) {
        cur = (cur + seg[i]) / 2;
    }

    cout << cur << endl;
    
    return 0;
}
```
