注意点：

1. 低于一万的数可以封装一个函数来打印，对于输入的数，按每四位一组，高位两组打印完后需要打印万和亿，前提对应的数不是 0
2. 打印 4 位数时，需要先去掉前导零，如果有需要打印一次零，打印剩余部分时，如果遇到后缀为若干 0 的，如果不是全 0 的情况就不用打印了直接退出
3. 还要注意一下格式问题，不要有多余的空格字符

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

char s[][20] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
char t[][20] = {"", "Shi", "Bai", "Qian", "Wan", "Yi"};

void print(string digit) {
    bool flag = false;
    while (digit.size() > 1 && digit[0] == '0') {
        digit = digit.substr(1);
        flag = true;
    }
    if (flag) cout << "ling ";
    for (int i = digit.size() - 1, j = 0; i >= 0; i--, j++) {
        string tmp;
        for (int k = j; k < digit.size(); k++) tmp += digit[k];
        if (stoi(tmp) == 0 && tmp.size() != digit.size()) break;
        
        cout << s[digit[j] - '0'];
        
        if (i > 0) {
            if (digit[j] != '0') {
                cout << " " << t[i];
                if (stoi(tmp.substr(1)) != 0) cout << " ";
            } else {
                cout << " ";
            }
        }
    }
}

int main() {
    int x;
    cin >> x;

    if (x < 0) {
        x = -x;
        cout << "Fu ";
    }

    string s = to_string(x);
    reverse(s.begin(), s.end());
    
    vector<string> vec;

    string tmp;
    for (int i = 0; i < s.size(); i++) {
        tmp.push_back(s[i]);
        if (tmp.size() == 4) {
            reverse(tmp.begin(), tmp.end());
            vec.push_back(tmp);
            tmp.clear();
        }
    }

    if (tmp.size() > 0) {
        reverse(tmp.begin(), tmp.end());
        vec.push_back(tmp);
    }

    for (int i = vec.size() - 1; i >= 0; i--) {
        if (vec.size() > 1 && stoi(vec[i]) == 0) continue;
        print(vec[i]);
        if (i > 0 && stoi(vec[i]) > 0) {
            cout << " " << t[i + 3] << " ";
        }
    }

    cout << endl;
    
    return 0;
}
```
