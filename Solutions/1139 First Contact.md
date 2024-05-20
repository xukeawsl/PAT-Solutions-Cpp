注意点：

1. 输入的 ID 可能有正负 0 的情况，因此不能直接通过整形值是否大于 0 来判断性别，要通过字符串形式读取然后看第一个字符
2. 输出的时候注意格式，前导 0 需要打印出来
3. 题目要求对于 a -> b 存在一条路径 a -> u -> v -> b，其中 u 和 a 性别相同， v 和 b 性别相同，因此可以不使用回溯，只需要暴力遍历 u 和 v 可能的值即可

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cstring>
#include <cstdio>
#include <unordered_map>
#include <unordered_set>

using namespace std;

unordered_map<int, unordered_set<int>> graph;
int g[10000];

int n, m, k;

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        string sa, sb;
        cin >> sa >> sb;
        int a, b;
        a = abs(stoi(sa));
        b = abs(stoi(sb));
        if (sa[0] == '-') g[a] = -1;
        else g[a] = 1;
        if (sb[0] == '-') g[b] = -1;
        else g[b] = 1;
        graph[a].emplace(b);
        graph[b].emplace(a);
    }

    cin >> k;
    for (int i = 0; i < k; i++) {
        int a, b;
        cin >> a >> b;
        a = abs(a), b = abs(b);

        vector<pair<int, int>> ans;
        for (int u : graph[a]) {
            if (g[u] != g[a] || u == a) continue;

            for (int v : graph[b]) {
                if (g[v] != g[b] || v == b) continue;

                if (u != v && u != b && v != a && graph[u].count(v)) {
                    ans.emplace_back(u, v);
                }
            }
        }
        
        sort(ans.begin(), ans.end());
        cout << ans.size() << endl;
        for (auto& [one, second] : ans) {
            printf("%04d %04d\n", one, second);
        }
    }
    
    return 0;
}

```
