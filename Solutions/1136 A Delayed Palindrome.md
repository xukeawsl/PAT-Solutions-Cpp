注意点：

1. 如果所给的数字本身就是回文的就不用进行运算了

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

string getAdd(const string& a, const string& b) {
    string ans;
    int d = 0;
    for (int i = 0; i < a.size(); i++) {
        d += (a[i] - '0') + (b[i] - '0');
        ans.push_back((d % 10) + '0');
        d /= 10;
    }
    if (d) ans.push_back(d + '0');
    reverse(ans.begin(), ans.end());
    return ans;
}

bool isPalindromic(const string& s) {
    int l = 0, r = s.size() - 1;
    while (l < r) {
        if (s[l] != s[r]) return false;
        l++;
        r--;
    }
    return true;
}

int main() {
    string digit;

    cin >> digit;

    bool flag = false;

    for (int i = 0; i < 10; i++) {
        if (isPalindromic(digit)) {
            flag = true;
            break;
        }
        string origin = digit;
        cout << digit << " + ";
        reverse(digit.begin(), digit.end());
        cout << digit << " = ";
        digit = getAdd(origin, digit);
        cout << digit << endl;
        if (isPalindromic(digit)) {
            flag = true;
            break;
        }
    }

    if (flag) {
        cout << digit << " is a palindromic number." << endl;
    } else {
        cout << "Not found in 10 iterations." << endl;
    }

    return 0;
}
```
