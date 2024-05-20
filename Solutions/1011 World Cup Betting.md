注意点：输入输出格式

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int main() {
    double w, t, l;
    double profit = 1.0;
    
    while (scanf("%lf %lf %lf", &w, &t, &l) != EOF) {
        double m = max(w, max(t, l));
        profit *= m;
        if (m == w) cout << "W ";
        else if (m == t) cout << "T ";
        else cout << "L ";
    }

    profit = (profit * 0.65 - 1) * 2;

    printf("%.2lf\n", profit);
    
    return 0;
}
```
