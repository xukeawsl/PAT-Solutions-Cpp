注意点：输入数据很大，需要用字符串读取

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    string input;
    cin >> input;

    int res = 0;
    for (char ch : input) {
        res += ch - '0';
    }

    string output = to_string(res);

    for (int i = 0; i < output.size(); i++) {
        switch (output[i]) {
            case '0': cout << "zero"; break;
            case '1': cout << "one"; break;
            case '2': cout << "two"; break;
            case '3': cout << "three"; break;
            case '4': cout << "four"; break;
            case '5': cout << "five"; break;
            case '6': cout << "six"; break;
            case '7': cout << "seven"; break;
            case '8': cout << "eight"; break;
            case '9': cout << "nine"; break;
        }
        if (i == output.size() - 1) cout << '\n';
        else cout << ' ';
    }
    
    return 0;
}
```
