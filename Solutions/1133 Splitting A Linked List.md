注意点：

1. 模拟链表的插入和删除，先将小于 0 的取出，然后将 0 ~ k 范围内的取出，剩余的放在最后
2. 注意存在全部元素都大于 k 的情况，因此要判断一下如果前面两段没有插入的节点时，使用最后一段链表

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int ne[100001], val[100001];

int main() {
    int first, n, k;
    cin >> first >> n >> k;
    
    for (int i = 0; i < n; i++) {
        int addr, data, next;
        cin >> addr >> data >> next;
        ne[addr] = next;
        val[addr] = data;
    }

    int dummpy = 100000;
    ne[dummpy] = first;

    int head = -1, t = -1;
    int p = dummpy, c = ne[dummpy];
    while (c != -1) {
        if (val[c] < 0) {
            if (head == -1) {
                head = c;
            } else {
                ne[t] = c;
            }
            t = c;
            ne[p] = ne[c];
            c = ne[p];
        } else {
            p = ne[p];
            c = ne[c];
        }
    }

    p = dummpy, c = ne[dummpy];
    while (c != -1) {
        if (val[c] >= 0 && val[c] <= k) {
            if (head == -1) {
                head = c;
            } else {
                ne[t] = c;
            }
            t = c;
            ne[p] = ne[c];
            c = ne[p];
        } else {
            p = ne[p];
            c = ne[c];
        }
    }

    if (head == -1) head = t = ne[dummpy];
    else ne[t] = ne[dummpy];

    while (head != -1) {
        if (ne[head] != -1) {
            printf("%05d %d %05d\n", head, val[head], ne[head]);
        } else {
            printf("%05d %d -1\n", head, val[head]);
        }
        head = ne[head];
    }
    
    return 0;
}
```
