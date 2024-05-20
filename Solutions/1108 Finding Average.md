注意点：

1. 浮点数还有数据范围和精度的要求
2. 打印的格式有三种情况

```cpp
#include <iostream>
#include <string>
#include <cstdio>
#include <numeric>

using namespace std;

bool check(const string& s) {
    int i = 0;
    if (s[0] == '+' || s[0] == '-') i++;
    int dot = 0;
    int tail = 0;
    while (i < s.size()) {
        if (!isdigit(s[i])) {
            if (s[i] == '.') {
                if (dot == 1) return false;
                dot = 1;
            } else {
                return false;
            }
        } else {
            if (dot == 1) tail++;
        }
        i++;
    }

    double value = stof(s);
    if (value < -1000 || value > 1000) return false;
    if (tail > 2) return false;
    return true;
}

int main() {
    int n;
    cin >> n;

    
    int cnt = 0;
    double sum = 0.0;
    for (int i = 0; i < n; i++) {
        string val;
        cin >> val;
        if (check(val)) {
            sum += stof(val);
            cnt++;
        } else {
            cout << "ERROR: " << val << " is not a legal number" << endl;
        }
    }

    if (cnt == 0) {
        cout << "The average of 0 numbers is Undefined" << endl;
    } else if (cnt == 1) {
        printf("The average of 1 number is %.2lf\n", sum);
    } else {
        printf("The average of %d numbers is %.2lf\n", cnt, sum / cnt);
    }
    
    return 0;
}
```
