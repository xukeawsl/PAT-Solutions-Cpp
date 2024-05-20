注意点：

1. 检查过的就打印 `Checked`，这里检查要是存在的学生

```cpp
#include <iostream>
#include <unordered_map>
#include <cstdio>
#include <unordered_set>

using namespace std;

bool isprime(int x) {
    if (x <= 1) return false;

    for (int i = 2; i <= x / i; i++) {
        if (x % i == 0) return false;
    }

    return true;
}

int main() {
    int n, k;
    cin >> n;
    
    unordered_map<int, int> rank;

    for (int i = 1; i <= n; i++) {
        int id;
        cin >> id;
        rank[id] = i;
    }

    cin >> k;

    unordered_set<int> st;

    for (int i = 0; i < k; i++) {
        int id;
        cin >> id;
        if (!rank.count(id)) {
            printf("%04d: Are you kidding?\n", id);
        } else {
            int r = rank[id];
            printf("%04d: ", id);
            if (st.count(id)) {
                printf("Checked\n");
            } else if (r == 1) {
                printf("Mystery Award\n");
            } else if (isprime(r)) {
                printf("Minion\n");
            } else {
                printf("Chocolate\n");
            }
            st.emplace(id);
        }
    }
    
    return 0;
}
```
