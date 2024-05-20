注意点：

1. 堆排序从小到大排序需要用到大根堆，每次将堆顶元素和最后一个元素交换后，已排序区域的大小扩大 1，未排序区域的大小减一
2. 建堆的过程就是从最后一个节点往前，对每个节点进行一次下滤操作

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int n;
int arr[101], tmp[101];
int target[101], ans[101];

bool InsertSort(int nums[]) {
    bool flag = false;

    for (int i = 2; i <= n; i++) {
        int j = i - 1;
        while (j >= 0 && nums[j] > nums[j + 1]) {
            swap(nums[j], nums[j + 1]);
            j--;
        }

        if (flag) {
            memcpy(ans, nums, sizeof(ans));
            return true;
        }

        if (!flag && memcmp(nums, target, sizeof(target)) == 0) {
            flag = true;
        }
    }

    return false;
}

void percdown(int nums[], int idx, int numsSize) {
    for (;;) {
        int left = idx * 2, right = idx * 2 + 1;
        int maxidx = idx;
        
        if (left <= numsSize && nums[left] > nums[maxidx]) maxidx = left;
        if (right <= numsSize && nums[right] > nums[maxidx]) maxidx = right;

        if (maxidx == idx) {
            break;
        }
        swap(nums[idx], nums[maxidx]);
        idx = maxidx;
    }
}

bool HeapSort(int nums[]) {
    for (int i = n / 2; i >= 1; i--) {
        percdown(nums, i, n);
    }

    bool flag = false;
    
    for (int i = n; i >= 1; i--) {
        swap(nums[1], nums[i]);
        percdown(nums, 1, i - 1);

        if (flag) {
            memcpy(ans, nums, sizeof(ans));
            return true;
        }

        if (!flag && memcmp(nums, target, sizeof(target)) == 0) {
            flag = true;
        }
    }

    return false;
}

int main() {
    cin >> n;

    for (int i = 1; i <= n; i++) cin >> arr[i];
    for (int i = 1; i <= n; i++) cin >> target[i];

    memcpy(tmp, arr, sizeof(arr));

    if (InsertSort(tmp)) {
        cout << "Insertion Sort" << endl;
    } else if (HeapSort(arr)) {
        cout << "Heap Sort" << endl;
    }

    for (int i = 1; i <= n; i++) {
        cout << ans[i];
        if (i != n) cout << ' ';
        else cout << '\n';
    }

    return 0;
}
```
