注意点：

1. 正向探测是起点是不变的，只是增量是两次方增量

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>

using namespace std;

int table[133331];
int msize, n, m;

bool isprime(int x) {
    if (x <= 1) return false;
    for (int i = 2; i <= x / i; i++) {
        if (x % i == 0) {
            return false;
        }
    }
    return true;
}

bool insert(int x) {
    int p = x % msize;
    if (table[p] == -1) {
        table[p] = x;
        return true;
    }

    int i = 1, npos = p;
    do {
        npos = (p + i * i) % msize;
        if (table[npos] == -1) {
            table[npos] = x;
            return true;
        }
        i++;
    } while (npos != p);

        
    return false;
}

int query(int x) {
    int cnt = 1;

    int p = x % msize;
    if (table[p] == x || table[p] == -1) {
        return cnt;
    }

    int i = 1, npos = p;
    do {
        npos = (p + i * i) % msize;
        cnt++;
        if (table[npos] == x || table[npos] == -1) {
            return cnt;
        }
        i++;
    } while (npos != p);

    
    return cnt;
}

int main() {
    memset(table, -1, sizeof(table));
    
    cin >> msize >> n >> m;
    while (!isprime(msize)) msize++;

    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        if (!insert(x)) {
            printf("%d cannot be inserted.\n", x);
        }
    }

    double total = 0;
    for (int i = 0; i < m; i++) {
        int x;
        cin >> x;
        total += query(x);
    }

    printf("%.1lf\n", total / m);
    
    return 0;
}
```
