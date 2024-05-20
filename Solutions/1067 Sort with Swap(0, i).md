注意点：

1. 贪心思路，如果 0 不在最终位置，则可以不断和 pos[0] 位置的元素交换，每一次交换一定能将一个数交换到最终位置，如果 0 在最终位置，但是剩余的仍然存在没有在对应位置的元素，则将 0 与其交换后再进入如上步骤，直到所有元素都排序成功
2. 还有一个方法就是统计每个环的元素个数，如果环中含有 0，则只需要交换 cnt - 1 即可，如果不含 0，则需要先将 0 换到环中，然后再进行上述交换，共 1 + (cnt + 1) - 1 即 cnt + 1 次交换

```cpp
#include <iostream>

using namespace std;

int pos[100001];

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        pos[x] = i;
    }

    int ans = 0;

    while (pos[0]) {
        swap(pos[0], pos[pos[0]]);
        ans++;
    }

    for (int i = 1; i < n; i++) {
        if (pos[i] == i) continue;
        swap(pos[0], pos[i]);
        ans++;
        while (pos[0]) {
            swap(pos[0], pos[pos[0]]);
            ans++;
        }
    }

    cout << ans << endl;
    
    return 0;
}
```
