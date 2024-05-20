注意点：

1. 题目要求通过关键字的绝对值去重
2. 去重的节点需要单独连接成一个链表

```cpp
#include <iostream>
#include <unordered_set>
#include <cstdio>

using namespace std;

bool st[10001];
int ne[100001], val[100001];

int main() {
    int n, head;
    cin >> head >> n;

    for (int i = 0; i < n; i++) {
        int addr, key, next;
        cin >> addr >> key >> next;
        ne[addr] = next;
        val[addr] = key;
    }

    int delhead = -1, deltail = -1;
    int dummpy = 100000;
    ne[dummpy] = head;

    int pre = dummpy, cur = head;
    while (cur != -1) {
        int key = abs(val[cur]);
        if (st[key]) {
            ne[pre] = ne[cur];
            if (delhead == -1) {
                delhead = cur;
                deltail = cur;
                ne[deltail] = -1;
            } else {
                ne[deltail] = cur;
                deltail = cur;
                ne[deltail] = -1;
            }
            cur = ne[pre];
        } else {
            st[key] = true;
            pre = ne[pre];
            cur = ne[pre];
        }
    }

    for (int h = ne[dummpy]; h != -1; h = ne[h]) {
        if (ne[h] != -1) {
            printf("%05d %d %05d\n", h, val[h], ne[h]);
        } else {
            printf("%05d %d %d\n", h, val[h], ne[h]);
        }
    }

    for (int h = delhead; h != -1; h = ne[h]) {
        if (ne[h] != -1) {
            printf("%05d %d %05d\n", h, val[h], ne[h]);
        } else {
            printf("%05d %d %d\n", h, val[h], ne[h]);
        }
    }
    
    return 0;
}
```
