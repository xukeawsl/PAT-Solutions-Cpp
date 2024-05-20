注意点：

1. 排序后连续的元素可能有重复的需要去掉

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int a[100001];
int i, n;

int main() {
    cin >> n;
    for (i = 0; i < n; i++) {
        cin >> a[i];
    }
    sort(a, a + n);

    int except = 1;

    i = 0;
    while (i < n && a[i] <= 0) i++;
    
    while (i < n && a[i] == except) {
        while (i < n && a[i] == except) i++;
        except++;
    }

    cout << except << endl;

    return 0;
}
```
