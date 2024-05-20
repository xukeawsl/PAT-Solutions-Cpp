注意点：

1. 归并排序的处理方式和一般的归并排序不太一样，是按照增量来自底向上来的，比如先按大小为 1 的，然后按大小为 2 的两两排序后再按大小为 3 的以此类推

```cpp
#include <iostream>
#include <vector>
#include <cstring>
#include <algorithm>

using namespace std;

int arr[101], tmp[101];
int target[101];
int ans[101];
int n;
bool flag;

bool insertSort(int nums[]) {
    for (int i = 1; i < n; i++) {
        for (int j = i; j > 0; j--) {
            if (nums[j - 1] > nums[j]) {
                swap(nums[j - 1], nums[j]);
            } else {
                break;
            }
        }

        if (flag) {
            memcpy(ans, nums, sizeof(ans));
            return true;
        }

        if (memcmp(nums, target, sizeof(target)) == 0) {
            flag = true;
        }
    }
    return false;
}

void mergeSort(int nums[], int l, int r) {
    if (l >= r) return;
    int mid = l + r >> 1;
    mergeSort(nums, l, mid);
    mergeSort(nums, mid + 1, r);

    int i = l, j = mid + 1, k = l;
    while (i <= mid && j <= r) {
        if (nums[i] <= nums[j]) tmp[k++] = nums[i++];
        else tmp[k++] = nums[j++];
    }
    while (i <= mid) tmp[k++] = nums[i++];
    while (j <= r) tmp[k++] = nums[j++];

    for (int i = l; i <= r; i++) nums[i] = tmp[i];
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) cin >> arr[i];
    for (int i = 0; i < n; i++) cin >> target[i];

    memcpy(tmp, arr, sizeof(arr));
    if (insertSort(tmp)) {
        cout << "Insertion Sort" << endl;
    } else {
        cout << "Merge Sort" << endl;

        for (int part = 1;; part *= 2) {
            int d = 0;
            while (d < n) {
                mergeSort(arr, d, min(d + part - 1, n - 1));
                d = min(d + part, n);
            }

            if (flag) {
                memcpy(ans, arr, sizeof(ans));
                break;
            }
    
            if (!flag && memcmp(arr, target, sizeof(target)) == 0) {
                flag = true;
            }
        }
    }

    for (int i = 0; i < n; i++) {
        cout << ans[i];
        if (i != n - 1) cout << ' ';
    }
    
    return 0;
}
```
