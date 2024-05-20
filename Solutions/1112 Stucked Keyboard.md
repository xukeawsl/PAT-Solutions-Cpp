注意点：

1. 只要有一段同字符子串长度不是 k 的整数倍就不算坏掉的键
2. 打印坏键按照检测的顺序来

```cpp
#include <iostream>
#include <string>
#include <unordered_set>

using namespace std;

int main() {
    int k;
    string input;

    cin >> k;
    getchar();

    getline(cin, input);

    int n = input.size();

    string stucked;
    string origin;

    unordered_set<char> normal;

    for (int i = 0; i < n;) {
        int j = i + 1;
        int cnt = 1;
        while (j < n && input[j] == input[i] && cnt < k) {
            cnt++;
            j++;
        }
        if (cnt < k) {
            normal.emplace(input[i]);
        }
        i = j;
    }

    unordered_set<char> st;

    for (int i = 0; i < n;) {
        if (normal.count(input[i])) {
            origin += input[i];
            i++;
        } else {
            if (!st.count(input[i])) {
                stucked += input[i];
                st.emplace(input[i]);
            }
            origin += input[i];
            i = i + k;
        }
    }

    cout << stucked << endl;
    cout << origin << endl;
    
    return 0;
}
```
