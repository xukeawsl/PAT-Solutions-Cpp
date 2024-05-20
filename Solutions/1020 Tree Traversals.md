注意点：

1. 后序遍历和中序遍历建树时，从右至左遍历，划分区间时先处理右半部分
2. 层序遍历输出时的格式要注意不要有多余的空格

```cpp
#include <iostream>
#include <unordered_map>
#include <queue>

using namespace std;

unordered_map<int, int> pos;
int n, idx;
int postorder[31], inorder[31];

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
};

TreeNode *buildTree(int l, int r) {
    if (l > r) return nullptr;

    TreeNode *node = new TreeNode;
    node->val = postorder[idx--];

    int p = pos[node->val];

    node->right = buildTree(p + 1, r);
    node->left = buildTree(l, p - 1);

    return node;
}

int main() {
    cin >> n;

    for (int i = 0; i < n; i++) cin >> postorder[i];
    
    for (int i = 0; i < n; i++) {
        cin >> inorder[i];
        pos[inorder[i]] = i;
    }

    idx = n - 1;
    TreeNode *root = buildTree(0, n - 1);

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int m = q.size();
        for (int i = 0; i < m; i++) {
            auto node = q.front(); q.pop();
            cout << node->val;

            if (i != m - 1) cout << ' ';
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        if (q.empty()) cout << '\n';
        else cout << ' ';
    }

    return 0;
}
```
