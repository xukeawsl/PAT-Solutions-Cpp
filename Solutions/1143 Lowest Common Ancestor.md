注意点：

1. 由于树节点值是唯一的，因此可以通过哈希表来建树，但是 AcWing 可能会卡哈希表，用在中序遍历序列中的下标来表示这个值
2. 求两个点的最近公共祖先，可以在建树的时候记录深度和每个节点对应的父节点，然后每次查询先将两个节点保持一致的高度，这时存在四种情况：1. 节点 u 在 v 之下，这时如果两者同高度时节点值相同，则 v 就是 u 的祖先节点；2. 情况同 1 类似；3.  u 和 v 本来就是同一个节点；4. u 和 v 同高度但是不是同一个节点，这时同步向上走，直到两点相同则就找到了最近公共祖先

```cpp
#include <iostream>
#include <unordered_map>
#include <algorithm>
#include <cstdio>

using namespace std;

const int N = 10001;

int m, n, idx;
int pre[N], in[N], index[N];
unordered_map<int, int> pos;
int l[N], r[N], p[N], d[N];

int buildTree(int left, int right, int depth) {
    if (left > right) return -1;

    int root = pre[idx++];
    int i = pos[root];
    
    d[i] = depth;
    l[i] = buildTree(left, i - 1, depth + 1);
    r[i] = buildTree(i + 1, right, depth + 1);

    if (l[i] != -1) p[l[i]] = i;
    if (r[i] != -1) p[r[i]] = i;
    
    return i;
}

int main() {
    cin >> m >> n;
    
    for (int i = 0; i < n; i++) {
        scanf("%d", &pre[i]);
        in[i] = pre[i];
    }

    sort(in, in + n);

    for (int i = 0; i < n; i++) {
        pos[in[i]] = i;
    }

    int root = buildTree(0, n - 1, 0);
    p[root] = root;

    while (m--) {
        int u, v;
        scanf("%d %d", &u, &v);

        if (!pos.count(u) && !pos.count(v)) {
            printf("ERROR: %d and %d are not found.\n", u, v);
            continue;
        } else if (!pos.count(u)) {
            printf("ERROR: %d is not found.\n", u);
            continue;
        } else if (!pos.count(v)) {
            printf("ERROR: %d is not found.\n", v);
            continue;
        }

        int tu = pos[u], tv = pos[v];
        if (d[tu] > d[tv]) {
            while (d[tu] > d[tv]) tu = p[tu];
            if (tu == tv) {
                printf("%d is an ancestor of %d.\n", v, u);
                continue;
            }
        } else if (d[tu] < d[tv]) {
            while (d[tu] < d[tv]) tv = p[tv];
            if (tu == tv) {
                printf("%d is an ancestor of %d.\n", u, v);
                continue;
            }
        } else {
            if (tu == tv) {
                printf("%d is an ancestor of %d.\n", u, v);
                continue;
            }
        }

        while (tu != tv) {
            tu = p[tu];
            tv = p[tv];
        }

        printf("LCA of %d and %d is %d.\n", u, v, in[tu]);
    }
    
    return 0;
}
```
