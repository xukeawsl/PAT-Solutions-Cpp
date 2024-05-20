注意点：

1. 当节点是叶子节点时不需要在两边加括号
2. 如果整棵树只有一个节点，则不需要给最后的结果取掉外层括号

```cpp
#include <iostream>
#include <string>

using namespace std;

string d[21];
int l[21], r[21], indegree[21];
int n;

string dfs(int root) {
    if (root == -1) return "";
    if (l[root] == -1 && r[root] == -1) return d[root];
    return "(" + dfs(l[root]) + d[root] + dfs(r[root]) + ")";
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> d[i] >> l[i] >> r[i];
        if (l[i] != -1) indegree[l[i]]++;
        if (r[i] != -1) indegree[r[i]]++;
    }

    int root;
    for (int i = 1; i <= n; i++) {
        if (indegree[i] == 0) {
            root = i;
            break;
        }
    }

    string ans = dfs(root);
    if (ans[0] == '(') {
        cout << ans.substr(1, ans.size() - 2) << endl;
    } else {
        cout << ans << endl;
    }
    
    return 0;
}
```
