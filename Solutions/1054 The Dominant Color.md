注意点：

1. 理解题目含义，主色占总面积的一半以上，因此统计每个像素的出现次数

```cpp
#include <iostream>
#include <unordered_map>

using namespace std;

int main() {
    int m, n;
    cin >> m >> n;

    unordered_map<int, int> cnt;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int color;
            cin >> color;
            cnt[color]++;
            if (cnt[color] * 2 > m * n) {
                cout << color << endl;
                return 0;
            }
        }
    }
    
    return 0;
}
```
