注意点：

1. 子序列和子数组不一样，不需要是连续的，因此顺序无关，可以进行排序，然后从左往右遍历使用双指针，右指针每轮都向右移动一步，然后调整左指针直到满足条件，取最大长度即可，注意返回的不是序列中的最大值而是序列最大数量
2. 考虑到数量范围，最小值乘以 p 可能超过 int 最大值，因此可以使用 long long

```cpp
#include <iostream>
#include <numeric>
#include <algorithm>
#include <vector>

using namespace std;

int main() {
    int n, p;
    cin >> n >> p;

    vector<long long> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    sort(arr.begin(), arr.end());

    int maxcnt = 0;
    
    for (int i = 0, j = 0; j < n; j++) {
        while (i < j && arr[j] > arr[i] * p) {
            i++;
        }
        if (maxcnt < j - i + 1) {
            maxcnt = j - i + 1;
        }
    }

    cout << maxcnt<< endl;
    
    return 0;
}
```
