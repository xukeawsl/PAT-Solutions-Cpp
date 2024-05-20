注意点：

1. 暴力求解即可，使用临界矩阵存图

```cpp
#include <iostream>

using namespace std;

bool g[201][201];
int v[101];

int main() {
    int nv, ne, m, k;
    cin >> nv >> ne;
    for (int i = 0; i < ne; i++) {
        int x, y;
        cin >> x >> y;
        g[x][y] = g[y][x] = true;
    }

    cin >> m;

    while (m--) {
        cin >> k;
        
        for (int i = 0; i < k; i++) cin >> v[i];

        bool isclque = true;
        for (int i = 0; i < k; i++) {
            if (!isclque) break;
            for (int j = i + 1; j < k; j++) {
                int x = v[i], y = v[j];
                if (!g[x][y]) {
                    isclque = false;
                    break;
                }
            }
        }

        if (!isclque) {
            cout << "Not a Clique" << endl;
            continue;
        }

        bool ismaximal = true;
        for (int i = 0; i < k; i++) {
            int u = v[i];
            for (int t = 1; t <= nv; t++) {
                if (g[u][t]) {
                    bool flag = true;
                    for (int j = 0; j < k; j++) {
                        if (i == j) {
                            continue;
                        }
                        if (t == v[j]) {
                            flag = false;
                            break;
                        }
                        if (!g[t][v[j]]) {
                            flag = false;
                        }
                    }
                    if (flag) {
                        ismaximal = false;
                        break;
                    }
                }
            }
        }

        if (ismaximal) {
            cout << "Yes" << endl;
        } else {
            cout << "Not Maximal" << endl;
        }
    }
    
    return 0;
}
```
