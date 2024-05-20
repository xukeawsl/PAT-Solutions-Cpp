注意点：

1. 每行的第一个数表示这行有个多少个元素，而不是元素的一部分

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;


int main() {
    long int n, x;

    vector<long int> vec;

    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> x;
        vec.push_back(x);
    }

    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> x;
        vec.push_back(x);
    }

    sort(vec.begin(), vec.end());

    cout << vec[(vec.size() - 1) / 2] << endl;
    
    
    return 0;
}
```
