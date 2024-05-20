注意点：

1. 使用虚拟头结点可以避免很多边界问题

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int p[100001], v[100001];

int main() {
    int first, n, k;

    scanf("%d %d %d", &first, &n, &k);

    for (int i = 0; i < n; i++) {
        int addr, data, next;
        scanf("%d %d %d", &addr, &data, &next);
        p[addr] = next;
        v[addr] = data;
    }

    int dummpy = 100000;
    p[dummpy] = first;

    int pre = dummpy, cur = dummpy;
    int cnt = 0;
    while (cur != -1) {
        if (cnt == k) {
            cnt = 1;

            int new_cur = p[cur];
            p[cur] = -1;

            int last = -1, head = p[pre], tail = p[pre];
            while (head != -1) {
                int tmp = p[head];
                p[head] = last;
                last = head;
                head = tmp;
            }

            p[pre] = last;
            p[tail] = new_cur;
            
            pre = tail;
            cur = new_cur;
        } else {
            cur = p[cur];
            cnt++;
        }
    }

    for (int node = p[dummpy]; node != -1; node = p[node]) {
        if (p[node] == -1) {
            printf("%05d %d %d\n", node, v[node], p[node]);
        } else {
            printf("%05d %d %05d\n", node, v[node], p[node]);
        }
    }
    
    return 0;
}
```
