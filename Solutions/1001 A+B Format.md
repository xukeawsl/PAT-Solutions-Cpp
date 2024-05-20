注意点：从右往左遍历，注意一下负号的情况即可

```cpp
#include <string>
#include <iostream>
#include <algorithm>

using namespace std;

int main() {
    int a, b;
    cin >> a >> b;
    string res = to_string(a + b);

    int n = res.size();
    string output;
    
    for (int i = n - 1, j = 0; i >= 0; i--, j++) {
        if (j == 3 && res[i] != '-') {
            output += ',';
            j = 0;
        }
        output += res[i];
    }

    reverse(output.begin(), output.end());

    cout << output << endl;
    
    return 0;
}
```
