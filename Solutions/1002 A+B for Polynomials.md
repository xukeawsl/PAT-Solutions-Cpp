注意点：系数并没有说一定是正数，例如 `-3.2x^3` ，其系数就是 `-3.2`，指数是 `3`，当系数为 0 时不用输出，因此在判断时应该判断非 0 值而不是大于 0

```cpp
#include <cstdio>
#include <cmath>

int main() {
    int k, n;
    double a;
    double values[1001] = {0};
    
    while (scanf("%d", &k) != EOF) {
        while (k--) {
            scanf("%d %lf", &n, &a);
            values[n] += a;
        }
    }

    k = 0;
    for (int i = 1000; i >= 0; i--) {
        if (values[i]) {
            k++;
        }
    }

    printf("%d", k);

    for (int i = 1000; i >= 0; i--) {
        if (values[i]) {
            printf(" %d %.1lf", i, values[i]);
        }
    }

    putchar('\n');

    return 0;
}
```
