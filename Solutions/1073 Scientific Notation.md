注意点：

1. 根据指数的大小对小数点进行前移后移，不足的先补 0，如果交换完后小数点位于最后一位，需要去除

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    string input;

    cin >> input;

    char sign = input[0];

    int k = input.find('E');
    
    int exp = stoi(input.substr(k + 1));

    input = input.substr(1, k - 1);

    k = input.find('.');

    if (exp < 0) {
        if (k < abs(exp) + 1) {
            input = string(abs(exp) + 1 - k, '0') + input;
            k = input.find('.');
        }
        for (int i = 0; i < abs(exp); i++) {
            swap(input[k], input[k - 1]);
            k--;
        }
    } else {
        if (input.size() - k < exp + 1) {
            input = input + string(exp + 1 - (input.size() - k), '0');
            k = input.find('.');
        }
        for (int i = 0; i < exp; i++) {
            swap(input[k], input[k + 1]);
            k++;
        }
    }

    if (input.back() == '.') input.pop_back();

    if (sign == '-') cout << sign;
    cout << input << endl;
    
    return 0;
}
```
