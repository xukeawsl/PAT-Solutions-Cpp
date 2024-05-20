注意点：

1. 考察 AVL 树的插入、层序遍历以及完全二叉树的判断
2. 可以在层序遍历时同时判断是否是完全二叉树，通过索引值来判断，对于完全二叉树，只需要判断节点对应的索引值是否大于节点数即可（下标从 1 开始）

```cpp
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;
const int N = 30;
int l[N], r[N], v[N], h[N], idx;

void update(int u)//更新以u为根的树的最大高度
{
    h[u] = max(h[l[u]], h[r[u]]) + 1;
}

void R(int& u)//右旋
{
    int p = l[u];
    l[u] = r[p], r[p] = u;
    update(u), update(p);
    u = p;
}

void L(int& u)//左旋
{
    int p = r[u];
    r[u] = l[p], l[p] = u;
    update(u), update(p);
    u = p;
}

int get_balance(int u)
{
    return h[l[u]] - h[r[u]];//左儿子高度减去右儿子高度
}

void insert(int& u, int w)
{
    if (!u) u = ++ idx, v[u] = w;//如果u=0表示没创建节点，就创建一个
    else if (w < v[u])//权重比根小
    {
        insert(l[u], w);//权重比根小的放左边
        if (get_balance(u) == 2)//如果左儿子比右儿子多两个节点
        {
            if (get_balance(l[u]) == 1) R(u);//u右旋
            else L(l[u]), R(u);//u的左儿子左旋，u右旋
        }
    }
    else//权重比根大
    {
        insert(r[u], w);
        if (get_balance(u) == -2)//如果右儿子比左儿子多两个节点
        {
            if (get_balance(r[u]) == -1) L(u);
            else R(r[u]), L(u);
        }
    }
    update(u);//更新以u为根的树的最大高度
}

int main()
{
    int n, root = 0;
    cin >> n;

    for (int i = 0; i < n; i++) {
        int w;
        cin >> w;
        insert(root, w);
    }

    queue<pair<int, int>> q;
    q.emplace(root, 1);
    bool flag = true;
    while (!q.empty()) {
        auto [u, idx] = q.front(); q.pop();
        if (idx > n) flag = false;
        cout << v[u];
        
        if (l[u]) q.emplace(l[u], 2 * idx);
        if (r[u]) q.emplace(r[u], 2 * idx + 1);
        
        if (q.empty()) cout << '\n';
        else cout << ' ';
    }
    
    if (flag) cout << "YES" << endl;
    else cout << "NO" << endl;
    
    return 0;
}
```
