注意点：

1. 进位的标准，参照示例

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int main() {
    int g1, s1, k1;
    int g2, s2, k2;
    int carry = 0;

    scanf("%d.%d.%d %d.%d.%d", &g1, &s1, &k1, &g2, &s2, &k2);
    int g, s, k;
    k = (k1 + k2) % 29;
    carry += (k1 + k2) / 29;
    s = (s1 + s2 + carry) % 17;
    carry = (s1 + s2 + carry) / 17;
    g = g1 + g2 + carry;

    printf("%d.%d.%d\n", g, s, k);
    return 0;
}
```
