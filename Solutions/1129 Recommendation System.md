注意点：

1. 使用自定义排序规则的 set 维护值和对应值的集合，词频高的在前，相同词频时值小的在前

```cpp
#include <iostream>
#include <unordered_map>
#include <set>
#include <cstdio>

using namespace std;

int main() {
    int n, k;
    scanf("%d %d", &n, &k);

    using PII = pair<int, int>;
    
    auto cmp = [](const PII& p1, const PII& p2) {
        if (p1.first == p2.first) {
            return p1.second < p2.second;
        }
        return p2.first < p1.first;
    };

    unordered_map<int, int> freq;
    set<PII, decltype(cmp)> st(cmp);

    for (int i = 1; i <= n; i++) {
        int x;
        scanf("%d", &x);
        if (i == 1) {
            freq[x] = 1;
            st.emplace(1, x);
        } else {
            printf("%d:", x);
            int t = min(i - 1, k);
            for (auto [_, v] : st) {
                if (t == 0) break;
                printf(" %d", v);
                t--;
            }
            putchar('\n');
            
            auto it = st.find({freq[x], x});
            if (it != st.end()) st.erase(it);
            freq[x]++;
            st.emplace(freq[x], x);
        }
    }

    return 0;
}
```
