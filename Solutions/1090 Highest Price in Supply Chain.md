注意点：

1. 建图+深度优先搜索即可

```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

double hp;
int n, cnt;
double p, r;
vector<bool> st;
vector<vector<int>> graph;

void dfs(int u, double price) {
    if (price > hp) {
        hp = price;
        cnt = 1;
    } else if (price == hp) {
        cnt++;
    }

    st[u] = true;

    for (int v : graph[u]) {
        if (!st[v]) dfs(v, price + price * r * 0.01);
    }
}

int main() {
    scanf("%d %lf %lf", &n, &p, &r);

    st.resize(n);
    graph.resize(n);
    
    int root;
    for (int i = 0; i < n; i++) {
        int u;
        scanf("%d", &u);
        if (u == -1) root = i;
        else {
            graph[u].emplace_back(i);
        }
    }

    dfs(root, p);

    printf("%.2lf %d\n", hp, cnt);
    
    return 0;
}
```
