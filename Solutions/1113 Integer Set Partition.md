注意点：

1. 排序后对半分即可

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    sort(arr.begin(), arr.end());
    int s1 = 0, s2 = 0;
    for (int i = 0; i < n; i++) {
        if (i < n / 2) s1 += arr[i];
        else s2 += arr[i];
    }
    if (n % 2 == 0) cout << "0 ";
    else cout << "1 ";
    cout << abs(s1 - s2) << endl;
    return 0;
}
```
