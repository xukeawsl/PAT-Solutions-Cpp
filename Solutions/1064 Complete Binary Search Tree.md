注意点：

1. 利用 BST 中序遍历有序的性质，先进行排序然后再利用数组存储完全二叉树的性质来复原完全二叉树，其层序遍历就是将数组遍历一次比较方便
2. 还有一种思路就是朴素建树，首先需要找到根节点，对于指定的区间，计算出左子树的节点数，则划分点就是左边界下标+左子树的节点数，求节点数要分两种情况，对于刚好有 2^k - 1 个节点的子树，直接取剩余总节点数的一半即可，对于超过 2^k - 1 但低于 2^(k + 1) - 1 个节点的情况，左边需要添加的节点数为第 k + 1 层可以添加的节点的一半和总结点数减去 2^k - 1 个节点的一半中的最小值

```cpp
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

vector<int> input;
vector<int> bst;
int n, k;

void inorder(int u) {
    if (u > n) return;
    inorder(2 * u);

    bst[u] = input[k++];
    
    inorder(2 * u + 1);
}

int main() {
    cin >> n;
    input.resize(n);
    bst.resize(n + 1);
    for (int i = 0; i < n; i++) {
        cin >> input[i];
    }
    sort(input.begin(), input.end());

    inorder(1);

    for (int i = 1; i <= n; i++) {
        cout << bst[i];
        if (i == n) cout << '\n';
        else cout << ' ';
    }
    
    return 0;
}
```
