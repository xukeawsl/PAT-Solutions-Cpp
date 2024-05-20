注意点：

1. 题目不要求把整个序列求出来，因此我们只关注给定的数是否含有一串连续的因子即可，遍历所有的起始因子，由于对称性，只需要遍历到根号即可，对于每个因此，尝试向后扩展，记录最大长度的结果
2. 如果结果数组为空，说明给定的数为 1 或质数，这时打印数本身即可

```cpp
#include <iostream>
#include <vector>
#include <cmath>

using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> ans;
    
    for (int i = 2; i <= sqrt(n); i++) {
        if (n % i == 0) {
            int m = n, j = i;
            vector<int> tmp;
            while (m % j == 0) {
                tmp.emplace_back(j);
                m /= j;
                j++;
            }
            if (tmp.size() > ans.size()) ans = tmp;
        }
    }

    if (ans.empty()) {
        ans.emplace_back(n);
    }

    cout << ans.size() << endl;
    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i];
        if (i != ans.size() - 1) cout << '*';
        else cout << '\n';
    }

    return 0;
}
```
