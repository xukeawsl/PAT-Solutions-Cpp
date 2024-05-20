注意点：

1. 求排除指定节点后的联通分量数，因为题目没有说明一开始全部联通，因此需要遍历所有可能的节点，最后的结果就是联通分量数减一
2. 有些平台卡输入，比如 AcWing，需要使用 scanf 来读取

```cpp
#include <vector>
#include <algorithm>
#include <cstdio>

using namespace std;

vector<bool> st;
vector<vector<int>> graph;

void dfs(int u) {
    st[u] = true;
    for (auto v : graph[u]) {
        if (!st[v]) dfs(v);
    }
}

int main() {
    int n, m, k;
    scanf("%d %d %d", &n, &m, &k);

    st.resize(n + 1);
    graph.resize(n + 1);
    
    for (int i = 0; i < m; i++) {
        int a, b;
        scanf("%d %d", &a, &b);
        graph[a].push_back(b);
        graph[b].push_back(a);
    }

    for (int i = 0; i < k; i++) {
        int u;
        scanf("%d", &u);
        int cnt = 0;
        fill(st.begin(), st.end(), 0);
        st[u] = true;
        for (int v = 1; v <= n; v++) {
            if (st[v]) continue;
            dfs(v);
            cnt++;
        }
        printf("%d\n", cnt - 1);
    }
    
    return 0;
}
```
