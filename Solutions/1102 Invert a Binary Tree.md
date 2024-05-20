注意点：

1. 题目要求先反转二叉树然后再求层序遍历和中序遍历

```cpp
#include <iostream>
#include <string>
#include <queue>

using namespace std;

int l[11], r[11];
int indegree[11];
int n, cnt;

void inverte(int root) {
    if (root == -1) return;

    inverte(l[root]);
    inverte(r[root]);

    swap(l[root], r[root]);
}

void inorder(int root) {
    if (root == -1) return;
    inorder(l[root]);
    cout << root;
    ++cnt;
    if (cnt == n) cout << '\n';
    else cout << ' ';
    inorder(r[root]);
}

int main() {
    cin >> n;

    for (int i = 0; i < n; i++) {
        string a, b;
        cin >> a >> b;
        if (a[0] == '-') l[i] = -1;
        else {
            l[i] = stoi(a);
            indegree[l[i]]++;
        }
        if (b[0] == '-') r[i] = -1;
        else {
            r[i] = stoi(b);
            indegree[r[i]]++;
        }
    }

    int root;
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) {
            root = i;
            break;
        }
    }

    inverte(root);

    queue<int> q;
    q.emplace(root);

    while (!q.empty()) {
        int u = q.front(); q.pop();
        if (l[u] != -1) q.emplace(l[u]);
        if (r[u] != -1) q.emplace(r[u]);
        cout << u;
        if (q.empty()) cout << '\n';
        else cout << ' ';
    }

    inorder(root);
    
    return 0;
}
```
