注意点：

1. 二叉树可以用数组形式表示，如果根的下标从 0 开始，则其左儿子对应的下标为 `2 * x + 1`，右儿子的下标为 `2 * x + 2` ，如果下标从 1 开始，则左儿子对应的下标为 `2 * x` ，右儿子的下标为 `2 * x + 1`
2. 如果下标从 0 开始，则对于完全二叉树来说，不会有下标超过节点数 - 1 的

```cpp
#include <iostream>
#include <cstdio>
#include <string>

using namespace std;

int l[21], r[21], indegree[21];
int t[21];
int n;
bool flag = true;

void dfs(int root, int idx) {
    if (idx >= n) {
        flag = false;
        return;
    }

    t[idx] = root;

    if (l[root] != -1) dfs(l[root], 2 * idx + 1);
    if (r[root] != -1) dfs(r[root], 2 * idx + 2);
}

int main() {
    cin >> n;

    int root;
    
    for (int i = 0; i < n; i++) {
        string s1, s2;
        cin >> s1 >> s2;
        if (s1[0] == '-') l[i] = -1;
        else {
            l[i] = stoi(s1);
            indegree[l[i]]++;
        }
        if (s2[0] == '-') r[i] = -1;
        else {
            r[i] = stoi(s2);
            indegree[r[i]]++;
        }
    }

    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) {
            root = i;
            break;
        }
    }

    dfs(root, 0);

    if (flag) {
        cout << "YES " << t[n - 1] << endl;
    } else {
        cout << "NO " << root << endl;
    }
    
    return 0;
}
```
