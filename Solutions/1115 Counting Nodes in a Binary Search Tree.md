注意点：

1. 考察二叉搜索树的插入，由于存在相同值的节点，因此无法用数组来模拟了

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void insert(TreeNode* root, int x) {
    if (x <= root->val) {
        if (root->left) {
            insert(root->left, x);
        } else {
            root->left = new TreeNode(x);
        }
    } else {
        if (root->right) {
            insert(root->right, x);
        } else {
            root->right = new TreeNode(x);
        }
    }
}

int main() {
    int n;
    cin >> n;

    int x;
    cin >> x;

    TreeNode* root = new TreeNode(x);
    
    for (int i = 0; i < n - 1; i++) {
        cin >> x;
        insert(root, x);
    }

    vector<int> cnt;
    queue<TreeNode*> q;
    q.emplace(root);

    while (!q.empty()) {
        int s = q.size();
        for (int i = 0; i < s; i++) {
            auto node = q.front(); q.pop();
            if (node->left) q.emplace(node->left);
            if (node->right) q.emplace(node->right);
        }
        cnt.emplace_back(s);
    }

    int n1 = cnt.back();
    int n2 = cnt[cnt.size() - 2];

    cout << n1 << " + " << n2 << " = " << n1 + n2;
    
    return 0;
}
```
