注意点：

1. 使用二次探测（Quadratic probing）的方法来解决哈希冲突，当增量不为 0 时如果回到了最初的起点，则表示无法再插入当前元素
2. 哈希表取不小于题目给定大小的素数

```cpp
#include <iostream>
#include <vector>
#include <cmath>
#include <cstring>

using namespace std;

vector<int> table;
int msize, n;

bool isprime(int x) {
    if (x <= 1) return false;
    for (int i = 2; i <= sqrt(x); i++) {
        if (x % i == 0) {
            return false;
        }
    }
    return true;
}

int insert(int key) {
    int pos = key % msize;

    for (int i = 0; ; i++) {
        int npos = (pos + i * i) % msize;
        if (table[npos] == -1) {
            table[npos] = key;
            return npos;
        }
        if (i != 0 && npos == pos) break;
    }
    
    return -1;
}

int main() {

    cin >> msize >> n;

    while (!isprime(msize)) {
        ++msize;
    }

    table.resize(msize, -1);

    for (int i = 0; i < n; i++) {
        int key;
        cin >> key;
        int p = insert(key);
        if (p == -1) cout << '-';
        else cout << p;

        if (i != n - 1) cout << ' ';
        else cout << '\n';
    }
    
    return 0;
}
```
