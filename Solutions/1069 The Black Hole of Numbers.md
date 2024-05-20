注意点：

1. 题目给定的输入可能不足 4 位，需要手动补 0

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <cstdio>

using namespace std;

int main() {
    string a;
    cin >> a;

    if (a.size() != 4) {
        a = string(4 - a.size(), '0') + a;
    }

    do {
        string b = a;
        sort(a.begin(), a.end(), greater<char>());
        sort(b.begin(), b.end());

        if (a == b) {
            cout << a << " - " << b << " = 0000" << endl;
            break;
        }

        int diff = stoi(a) - stoi(b);
        char num[10] = {0};
        sprintf(num, "%04d", diff);

        cout << a << " - " << b << " = " << num << endl;

        a = num;
        
    } while (a != "6174");
    
    return 0;
}
```
