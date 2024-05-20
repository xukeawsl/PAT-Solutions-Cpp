注意点：

1. 分情况判断即可

```cpp
#include <iostream>

using namespace std;

int n, m, k;
int heap[1001], pos[1001];

bool isMaxHeap(int idx) {
    if (idx > n) return true;

    if (idx * 2 <= n && heap[idx * 2] > heap[idx]) return false;
    if (idx * 2 + 1 <= n && heap[idx * 2 + 1] > heap[idx]) return false;

    return isMaxHeap(idx * 2) && isMaxHeap(idx * 2 + 1);
}

bool isMinHeap(int idx) {
    if (idx > n) return true;

    if (idx * 2 <= n && heap[idx * 2] < heap[idx]) return false;
    if (idx * 2 + 1 <= n && heap[idx * 2 + 1] < heap[idx]) return false;

    return isMinHeap(idx * 2) && isMinHeap(idx * 2 + 1);
}

void dfs(int idx) {
    if (idx > n) return;
    dfs(idx * 2);
    dfs(idx * 2 + 1);
    pos[k++] = heap[idx];
}

int main() {
    cin >> m >> n;
    for (int i = 0; i < m; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> heap[j];
        }

        if (isMaxHeap(1)) {
            cout << "Max Heap" << endl;
        } else if (isMinHeap(1)) {
            cout << "Min Heap" << endl;
        } else {
            cout << "Not Heap" << endl;
        }

        k = 0;
        dfs(1);

        for (int j = 0; j < k; j++) {
            cout << pos[j];
            if (j != k - 1) cout << ' ';
            else cout << '\n';
        }
    }
    return 0;
}
```
