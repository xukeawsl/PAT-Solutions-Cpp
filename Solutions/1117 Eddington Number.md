注意点：

1. 不需要选择连续的，因此可以倒序排序后依次遍历，如果当前元素大于天数则取一次结果

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> ride(n);

    for (int i = 0; i < n; i++) {
        cin >> ride[i];
    }

    sort(ride.begin(), ride.end(), greater<int>());

    int ans = 0;
    for (int i = 0; i < n; i++) {
        if (ride[i] > i + 1) {
            ans = i + 1;
        }
    }

    cout << ans << endl;
    
    return 0;
}
```
