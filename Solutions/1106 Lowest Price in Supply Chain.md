注意点：

1. 统计的是到达叶子节点处，所能得到的最小价格

```cpp
#include <iostream>
#include <vector>
#include <cstdio>

using namespace std;

int n;
double p, r;
vector<vector<int>> grpah;

double ans = 1e20;
int number;

void dfs(int u, double price) {
    if (grpah[u].empty()) {
        if (price < ans) {
            number = 1;
            ans = price;
        } else if (price == ans) {
            number++;
        }
    }

    for (int v : grpah[u]) {
        dfs(v, price + price * r * 0.01);
    }
}

int main() {
    cin >> n >> p >> r;
    grpah.resize(n);
    
    for (int i = 0; i < n; i++) {
        int cnt = 0;
        cin >> cnt;
        for (int j = 0; j < cnt; j++) {
            int v;
            cin >> v;
            grpah[i].emplace_back(v);
        }
    }

    dfs(0, p);

    printf("%.4lf %d\n", ans, number);
    
    return 0;
}
```
