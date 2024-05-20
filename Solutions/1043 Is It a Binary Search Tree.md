注意点：

1. 给定二叉搜索树的前序遍历，那么其中序遍历就是已知的，排序即可
2. 由于需要判断是否是有效的二叉搜索树，我们尝试通过前序遍历和中序遍历构建，注意不能用哈希表存中序遍历下标，因为存在重复元素，故只能从左往右依次遍历，这样可以保证大于等于的数在右侧（左子树通过左侧区间构建，右子树通过右侧区间构建）
3. 构建不需要真正构建一颗二叉树，只需模拟即可，返回的结果表示能否成功构建，如果构建失败，则给定序列可能是二叉搜索树镜像之后的，这时采用另一种镜像模式构建（左子树通过右侧区间构建，右子树通过左侧区间构建）
4. 在构建的过程中我们可以顺便记录后序遍历的结果，注意第一次构建失败后需要清空上次的内容

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

int n;
vector<int> preorder;
vector<int> inorder;
vector<int> postorder;
int idx;

bool build(int l, int r, int type) {
    if (l > r) return true;

    int val = preorder[idx++];

    bool flag = false;
    
    for (int i = l; i <= r; i++) {
        if (inorder[i] == val) {
            if (type) {
                flag = build(l, i - 1, type) && build(i + 1, r, type);
            } else {
                flag = build(i + 1, r, type) && build(l, i - 1, type);
            }
            break;
        }
    }

    postorder.push_back(val);

    return flag;
}


int main() {
    cin >> n;
    preorder.resize(n);
    inorder.resize(n);
    for (int i = 0; i < n; i++) {
        cin >> preorder[i];
        inorder[i] = preorder[i];
    }
    
    sort(inorder.begin(), inorder.end());

    if (!build(0, n - 1, true)) {
        idx = 0;
        postorder.clear();
        if (build(0, n - 1, false)) {
            cout << "YES" << endl;
            for (int i = 0; i < n; i++) {
                cout << postorder[i];
                if (i != n - 1) cout << ' ';
                else cout << '\n';
            }
            return 0;
        } else {
            cout << "NO" << endl;
        }
    } else {
        cout << "YES" << endl;
        for (int i = 0; i < n; i++) {
            cout << postorder[i];
            if (i != n - 1) cout << ' ';
            else cout << '\n';
        }
        return 0;
    }
    
    return 0;
}
```
