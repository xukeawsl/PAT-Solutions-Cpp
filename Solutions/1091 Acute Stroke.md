注意点：

1. 理解题目含义是求连通分量大小不小于给定阈值的体积总大小
2. 根据图片可以知道上下左右上下的像素是连通的，因此是求三维的连通分量大小，由于 dfs 可能爆栈，使用 bfs 来求

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <tuple>

using namespace std;

int a[1287][129][61];
bool st[1287][129][61];
int m, n, l, t;
int ans;

const int dxyz[6][3] = {
    {-1, 0, 0}, {1, 0, 0}, {0, -1, 0},
    {0, 1, 0}, {0, 0, -1}, {0, 0, 1}
};

int bfs(int x, int y, int z) {
    queue<tuple<int, int, int>> q;
    q.emplace(x, y, z);
    st[x][y][z] = true;

    int cnt = 0;
    
    while (!q.empty()) {
        auto [x, y, z] = q.front(); q.pop();
        cnt++;
        for (int i = 0; i < 6; i++) {
            int dx = x + dxyz[i][0], dy = y + dxyz[i][1], dz = z + dxyz[i][2];
            if (dx < 0 || dx >= m || dy < 0 || dy >= n || dz < 0 || dz >= l ||
               a[dx][dy][dz] == 0 || st[dx][dy][dz]) continue;

            st[dx][dy][dz] = true;
            q.emplace(dx, dy, dz);
        }
    }

    return cnt;
}

int main() {
    cin >> m >> n >> l >> t;

    for (int k = 0; k < l; k++) {
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                cin >> a[i][j][k];
            }
        }
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < l; k++) {
                if (a[i][j][k] == 0) continue;
                if (st[i][j][k]) continue;

                int cnt = bfs(i, j, k);
                if (cnt >= t) {
                    ans += cnt;
                }
            }
        }
    }

    cout << ans << endl;
    
    return 0;
}
```
