注意点：

1. 将地球数字转火星数字时需要注意只有高位没有低位的情况

```cpp
#include <iostream>
#include <string>

using namespace std;

string low[] = 
{"tret", "jan", "feb", "mar", "apr", "may", "jun", "jly", "aug", "sep", "oct", "nov", "dec"};

string high[] = 
{"", "tam", "hel", "maa", "huh", "tou", "kes", "hei", "elo", "syy", "lok", "mer", "jou"};

int main() {
    int n;
    cin >> n;
    getchar();
    
    for (int i = 0; i < n; i++) {
        string input;
        getline(cin, input);

        if (isdigit(input[0])) {
            int num = stoi(input);
            int a = num / 13;
            int b = num % 13;
            if (a > 0) {
                cout << high[a];
                if (b > 0) cout << ' ' << low[b] << '\n';
                else cout << '\n';
            } else {
                cout << low[b] << '\n';
            }
        } else {
            int k = input.find(' ');
            string a, b;
            if (k != -1) {
                a = input.substr(0, k);
                b = input.substr(k + 1);
            } else {
                a = input;
                b = input;
            }

            int num = 0;
            for (int i = 1; i <= 12; i++) {
                if (high[i] == a) {
                    num += i * 13;
                    break;
                }
            }
            for (int i = 0; i < 13; i++) {
                if (low[i] == b) {
                    num += i;
                    break;
                }
            }
            cout << num << endl;
        }
    }

    return 0;
}
```
