注意点：

1. 必须按照给点的路径从 A->xxx->A，并且需要经过图中的所有节点且只有 A 出现两次
2. 因此给定路径的点数需要为节点数+1
3. 注意需要将输入数据都读完才行，不能处理到途中就提前返回

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n, m, k;
    cin >> n >> m;

    vector<vector<bool>> graph(n + 1, vector<bool>(n + 1));
    vector<bool> vis(n + 1);

    for (int i = 0; i < m; i++) {
        int x, y;
        cin >> x >> y;
        graph[x][y] = graph[y][x] = true;
    }

    cin >> k;

    for (int i = 0; i < k; i++) {
        fill(vis.begin(), vis.end(), false);
        int cnt, start, pre;
        cin >> cnt;
        bool flag = true;
        if (cnt != n + 1) flag = false;
        for (int j = 0; j < cnt; j++) 
        {
            int cur;
            cin >> cur;
            if (!flag) continue;
            if (j == 0) {
                pre = start = cur;
                vis[pre] = true;
                continue;
            }
            if (vis[cur]) {
                if (j != cnt - 1 || cur != start) {
                    flag = false;
                }
            }
            if (graph[pre][cur]) {
                pre = cur;
                vis[pre] = true;
            } else {
                flag = false;
            }
        }
        if (flag) cout << "YES" << endl;
        else cout << "NO" << endl;
    }

    return 0;
}
```
