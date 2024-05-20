注意点：

1. 此题如果暴力求中序遍历的所有序列来检查会超时
2. 由于只给中序遍历和后序遍历无法确认左子树和右子树的长度，因此只能暴力枚举左子树的长度了，对于前序遍历区间 `[l1, r1]`，很明显 `pre[l1]` 为根节点，左子树区间的左端点很明显为 `pre[l1 + 1]`，因此枚举区间右端点即可，右端点从 `l1` 开始枚举到 `r1`，这样可以包含左子树为空的情况，确认了左子树区间 `[l1 + 1, i]`，则右子树区间为 `[i + 1, r1]`，对应后序遍历的区间依次为 `[l2, l2 + (i - l1 - 1)]` 和 `[l2 + (i - l1), r2 - 1]`
3. 如果 `pre[l1]`不等于 `post[r2]` 则说明方案不成立，方案数为 0，对于每一层来说，方案数为左子树方案数×右子树方案数，可以同时求对应的合法中序遍历序列然后拼接起来即可

```cpp
#include <iostream>
#include <string>

using namespace std;

int n;
int preorder[31], postorder[31];

int dfs(int l1, int r1, int l2, int r2, string& s) {
    if (l1 > r1) return 1;
    if (preorder[l1] != postorder[r2]) return 0;

    int cnt = 0;
    for (int i = l1; i <= r1; i++) {
        string ls, rs;
        int lcnt = dfs(l1 + 1, i, l2, l2 + (i - l1 - 1), ls);
        int rcnt = dfs(i + 1, r1, l2 + (i - l1), r2 - 1, rs);

        if (lcnt && rcnt) {
            s = ls + to_string(preorder[l1]) + ' ' + rs;
            cnt += lcnt * rcnt;
            if (cnt > 1) {
                break;
            }
        }
    }

    return cnt;
}

int main() {
    cin >> n;

    for (int i = 0; i < n; i++) {
        cin >> preorder[i];
    }

    for (int i = 0; i < n; i++) {
        cin >> postorder[i];
    }

    string ans;
    int cnt = dfs(0, n - 1, 0, n - 1, ans);

    if (cnt > 1) {
        cout << "No" << endl;
    } else {
        cout << "Yes" << endl;
    }

    ans.pop_back();

    cout << ans << endl;
    
    return 0;
}
```
