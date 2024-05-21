注意点：

1. 如果使用暴力求法，每次求两个点的最近公共祖先的时间复杂度是 O(n)，而题目查询的次数 m 最多 1000，节点数最多 10000，因此时间复杂度为 O(nm) 为 1e7 的规模，在常数大的情况下可能超时，因此采用更优的 tarjan 算法
2. tarjan 算法是一种离线求解的算法，先要记住所有的查询，然后对树进行中序遍历，对于当前访问的节点 u 如果存在左右子节点就递归求解，然后统计 u 涉及到的查询，对于每一个 v，如果它已经被访问过，则 v 点当前对应的祖先节点也是 u 的祖先节点，对于节点 u，以它自身为根节点，其祖先节点就是它本身，递归完左子节点后，左子节点的最近祖先就是 u，右子节点同理，对于完成 tarjan 的节点，其祖先节点会被置为其父节点，这时在求解另一个节点的 tarjan 时，可以通过并查集来快速找到两者的最近公共祖先，此算法的时间复杂度为 O(n + m)

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <string>

using namespace std;

int m, n, idx;
int in[10001], pre[10001];
int l[10001], r[10001];
int ut[10001], vis[10001];
string ans[1001];

vector<pair<int, int>> query(1001);
vector<vector<pair<int, int>>> graph(10001);

unordered_map<int, int> pos;

int buildTree(int left, int right) {
    if (left > right) return -1;

    int p = pos[pre[idx++]];

    l[p] = buildTree(left, p - 1);
    r[p] = buildTree(p + 1, right);

    return p;
}

int find(int x) {
    if (ut[x] == x) return x;
    return ut[x] = find(ut[x]);
}

void tarjan(int u) {
    vis[u] = true;

    if (l[u] != -1) {
        tarjan(l[u]);
        ut[l[u]] = u;
    }

    if (r[u] != -1) {
        tarjan(r[u]);
        ut[r[u]] = u;
    }

    for (auto [v, i] : graph[u]) {
        if (vis[v]) {
            int lca = find(v);
            if (lca == v) {
                ans[i] = to_string(in[v]) + " is an ancestor of "
                         + to_string(in[u]) + ".";
            } else if (lca == u) {
                ans[i] = to_string(in[u]) + " is an ancestor of "
                         + to_string(in[v]) + ".";
            } else {
                ans[i] = "LCA of " + to_string(query[i].first) + " and "
                         + to_string(query[i].second) + " is " + to_string(in[lca]) + ".";
            }
        }
    }
}

int main() {
    cin >> m >> n;

    for (int i = 0; i < n; i++) {
        cin >> in[i];
        pos[in[i]] = i;
    }

    for (int i = 0; i < n; i++) {
        ut[i] = i;
        cin >> pre[i];
    }

    int root_idx = buildTree(0, n - 1);

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;

        query[i] = {u, v};

        if (!pos.count(u) && !pos.count(v)) {
            ans[i] = "ERROR: " + to_string(u) + " and "
                     + to_string(v) + " are not found.";
        } else if (!pos.count(u)) {
            ans[i] = "ERROR: " + to_string(u) + " is not found.";
        } else if (!pos.count(v)) {
            ans[i] = "ERROR: " + to_string(v) + " is not found.";
        } else {
            int idx_u = pos[u], idx_v = pos[v];
            graph[idx_u].emplace_back(idx_v, i);
            graph[idx_v].emplace_back(idx_u, i);
        }
    }

    tarjan(root_idx);

    for (int i = 0; i < m; i++) {
        cout << ans[i] << endl;
    }
    
    return 0;
}
```
