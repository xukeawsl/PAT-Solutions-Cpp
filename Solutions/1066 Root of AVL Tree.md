注意点：

1. 考察 AVL 树的插入

```cpp
#include <iostream>
#include <algorithm>
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

    while (n -- )
    {
        int w;
        cin >> w;
        insert(root, w);
    }

    cout << v[root];

    return 0;
}
```
