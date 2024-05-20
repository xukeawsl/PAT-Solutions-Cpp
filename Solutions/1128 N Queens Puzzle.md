注意点：

1. 棋盘的起始坐标在左下角，给定在每列的行号，判断是否满足条件，检查是否存在一行或对角线存在一个以上的皇后
2. 注意检查对角线时要覆盖全面

```cpp
#include <iostream>
#include <cstring>

using namespace std;

bool g[1001][1001];

bool check(int n) {
    for (int row = 1; row <= n; row++) {
        int cnt = 0;
        for (int col = 1; col <= n; col++) {
            if (g[row][col]) cnt++;
        }
        if (cnt > 1) return false;

        cnt = 0;
        int x = row, y = 1;
        while (x >= 1 && y <= n) {
            if (g[x][y]) cnt++;
            x--;
            y++;
        }
        if (cnt > 1) return false;

        cnt = 0;
        x = row, y = 1;
        while (x <= n && y <= n) {
            if (g[x][y]) cnt++;
            x++;
            y++;
        }
        if (cnt > 1) return false;
    }

    for (int col = 1; col <= n; col++) {
        int cnt = 0;
        int x = 1, y = col;
        while (x <= n && y <= n) {
            if (g[x][y]) cnt++;
            x++;
            y++;
        }
        if (cnt > 1) return false;

        cnt = 0;
        x = n, y = col;
        while (x >= 1 && y <= n) {
            if (g[x][y]) cnt++;
            x--;
            y++;
        }
        if (cnt > 1) return false;
    }
    

    return true;
}

int main() {
    int k;
    cin >> k;
    for (int i = 0; i < k; i++) {
        int n;
        cin >> n;
        memset(g, 0, sizeof(g));
        for (int col = 1; col <= n; col++) {
            int row;
            cin >> row;
            g[n - row + 1][col] = true;
        }
        
        if (check(n)) {
            cout << "YES" << endl;
        } else {
            cout << "NO" << endl;
        }
    }
    
    return 0;
}
```
