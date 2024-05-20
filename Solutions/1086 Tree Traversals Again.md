注意点：

1. 中序遍历可以非递归方式来实现，当 push 一个元素时，如果其父节点没有左儿子，则当前节点为其左儿子，否则为右儿子，每次 pop 一个元素时都更新父节点为当前 pop 的节点
2. 建树后后序遍历一下即可，注意根节点最后打印，因此可以在根节点后跟回车而不是空格

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <functional>

using namespace std;

int l[31], r[31];

int main() {
    int n;

    cin >> n;

    int root;
    int node = -1;
    stack<int> stk;
    
    for (int i = 0; i < 2 * n; i++) {
        string cmd;
        cin >> cmd;
        if (cmd == "Push") {
            int x;
            cin >> x;
            if (node == -1) {
                root = x;
                l[node] = x;
            } else {
                if (l[node]) r[node] = x;
                else l[node] = x;
            }
            stk.push(x);
            node = x;
        } else {
            node = stk.top();
            stk.pop();
        }
    }

    function<void(int)> dfs = [&](int u) {
        if (!u) return;
        
        dfs(l[u]);
        dfs(r[u]);
        
        if (u == root) {
            cout << u << '\n';
        } else {
            cout << u << ' ';
        }
    };

    dfs(root);

    return 0;
}
```
