注意点：读题要仔细，求最大子序列和，子序列是连续的，且题目要求输出的是子序列的第一个和最后一个数，不是输出下标，示例可能会产生误导，而且还有一种特殊情况，当全部都是负数时，最大和为 0 并输出整个序列的第一个和最后一个数，如果存在正数，则求下标对最小的序列，即序列的左侧和右侧下标尽可能小，如果和相同的情况下，还有一个坑就是最大和是可能为负数的，因此初始时用于比较的最大值需要设置为负数

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int k;
    cin >> k;

    vector<int> a(k);

    bool flag = true;
    
    for (int i = 0; i < k; i++) {
        cin >> a[i];
        if (a[i] >= 0) flag = false;
    }

    if (flag) {
        cout << 0 << ' ' << a[0] << ' ' << a[k - 1] << endl;
        return 0;
    }
    
    vector<int> sum(k + 1);

    for (int i = 1; i <= k; i++) sum[i] = sum[i - 1] + a[i - 1];

    int minval = 0, minp = 0;
    int maxsum = -1e9, left = 0, right = 0;
    
    for (int i = 1; i <= k; i++) {
        if (sum[i] - minval > maxsum) {
            maxsum = sum[i] - minval;
            left = minp, right = i - 1;
        }

        if (sum[i] < minval) {
            minval = sum[i];
            minp = i;
        }
    }

    cout << maxsum << ' ' << a[left] << ' ' << a[right] << endl;
    
    return 0;
}

```
