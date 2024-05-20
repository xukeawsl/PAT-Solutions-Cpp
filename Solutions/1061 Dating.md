注意点：

1. 日期从前两个字符中的第一个**同位相同**的**有效大写字母**获取，小时则从之后**同位相同**的**有效字符**取，0~9、A~N
2. 分钟则是从后两个字符串同位相同的**有效字母**取下标

```cpp
#include <iostream>
#include <string>
#include <cstdio>

using namespace std;

int cnt[256] = {0};

const char week[][4] = {"MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"};

int main() {
    string a, b, c, d;
    
    cin >> a >> b >> c >> d;

    int first = -1, second;
    int idx = 0;

    for (int i = 0; i < min(a.size(), b.size()); i++) {
        if (a[i] == b[i]) {
            if (first == -1) {
                if (isupper(a[i]) && a[i] - 'A' < 7) {
                    first = a[i] - 'A';
                }
            } else {
                if (isdigit(a[i]) || (isupper(a[i]) && a[i] <= 'N')) {
                    if (isdigit(a[i])) second = a[i] - '0';
                    else second = a[i] - 'A' + 10;
                    break;
                }
            }
        }
    }

    for (int i = 0; i < min(c.size(), d.size()); i++) {
        if (c[i] == d[i] && isalpha(c[i])) {
            idx = i;
            break;
        }
    }

    printf("%s %02d:%02d\n", week[first], second, idx);
    
    return 0;
}
```
