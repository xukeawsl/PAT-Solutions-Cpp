注意点：

1. 不考虑有环的情况
2. 求两个链表的第一个公共节点，可以用经典的同步走算法或哈希表计数，也可以用根据总距离相同的走法来确定

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int lst[100001];

int main() {
    int n;
    int f1, f2;
    scanf("%d %d %d", &f1, &f2, &n);

    for (int i = 0; i < n; i++) {
        int a, next;
        char d;
        scanf("%d %c %d", &a, &d, &next);
        lst[a] = next;
    }

    int p1 = f1, p2 = f2;
    while (p1 != p2) {
        if (p1 != -1) p1 = lst[p1];
        else p1 = f2;
        if (p2 != -1) p2 = lst[p2];
        else p2 = f1;
    }
    
    if (p1 == -1) printf("-1\n");
    else printf("%05d\n", p1);
    
    return 0;
}
```
