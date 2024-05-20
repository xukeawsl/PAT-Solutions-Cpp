注意点：

1. 将多段连接后得到的序列最小，排序时不能只按两个字符排序，而是将他们互相拼接的结果进行排序，这样长度相同的情况下如果 a + b < b + a，则 a 在前更优
2. 最后输出的结果需要去除前导 0

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n;
    cin >> n;
    vector<string> vec(n);
    for (int i = 0; i < n; i++) {
        cin >> vec[i];
    }

    sort(vec.begin(), vec.end(), [](const string& a, const string& b){
        return a + b < b + a;
    });

    string ans;

    for (auto& v : vec) ans += v;

    int i = 0;
    while (i < ans.size() - 1 && ans[i] == '0') i++;

    cout << ans.substr(i) << endl;
    

    return 0;
}
```
