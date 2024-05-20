注意点：

1. 生成螺旋矩阵，方向数组为顺时针，当指定方向不能再移动时才切换下一个方向

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int N;
    cin >> N;

    vector<int> data(N);

    for (int i = 0; i < N; i++) cin >> data[i];

    sort(data.begin(), data.end(), greater<int>());

    int m = N, n = 1;

    for (int i = 2; i <= N / i; i++) {
        if (N % i == 0) {
            m = max(i, N / i);
            n = min(i, N / i);
        }
    }

    vector<vector<int>> matrix(m, vector<int>(n, -1));

    const int dxy[4][2] = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    
    int x = 0, y = 0, d = 0;
    for (int i = 0; i < N; i++) {
        matrix[x][y] = data[i];

        for (int j = 0; j < 4; j++) {
            int dx = x + dxy[d][0], dy = y + dxy[d][1];
            if (dx < 0 || dx >= m || dy < 0 || dy >= n || matrix[dx][dy] != -1) {
                d = (d + 1) % 4;
                continue;
            }
            x = dx, y = dy;
            break;
        }
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cout << matrix[i][j];
            if (j != n - 1) cout << ' ';
            else cout << '\n';
        }
    }
    
    return 0;
}
```
