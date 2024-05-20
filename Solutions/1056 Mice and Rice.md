注意点：

1. 使用队列模拟时，进行比赛的前提是队列大小大于 1，这样就不会出现死循环的问题
2. 题目中的排名是依据每轮比赛的组合来的，而不是从一开始的组数依次倒数，且当轮淘汰的人排名相同且为组数 + 1

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

int main() {
    int np, ng;
    cin >> np >> ng;

    vector<int> weight(np);

    for (int i = 0; i < np; i++) cin >> weight[i];

    queue<int> q;

    for (int i = 0; i < np; i++) {
        int order;
        cin >> order;
        q.push(order);
    }

    vector<int> ans(np);
    
    while (q.size() > 1) {
        int n = q.size();
        int rank = (n + ng - 1) / ng + 1;
        int win = -1, cnt = 0;
        for (int i = 0; i < n; i++) {
            int idx = q.front(); q.pop();
            if (win == -1 || weight[idx] > weight[win]) {
                win = idx;
            }
            ans[idx] = rank;
            cnt++;
            if (cnt == ng || i == n - 1) {
                q.push(win);
                cnt = 0;
                win = -1;
            }
        }
    }

    ans[q.front()] = 1;

    for (int i = 0; i < np; i++) {
        cout << ans[i];
        if (i == np - 1) cout << '\n';
        else cout << ' ';
    }
    
    return 0;
}
```
