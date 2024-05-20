注意点：

1. 洗牌是通过映射的方式而不是交换

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <utility>
#include <numeric>

using namespace std;

string getCard(int d) {
    if (d >= 1 && d <= 13) {
        return "S" + to_string(d);
    } else if (d >= 14 && d <= 26) {
        return "H" + to_string(d - 13);
    } else if (d >= 27 && d <= 39) {
        return "C" + to_string(d - 26);
    } else if (d >= 40 && d <= 52) {
        return "D" + to_string(d - 39);
    }
    return "J" + to_string(d - 52);
}

int main() {
    int k;
    cin >> k;

    vector<int> idx(54);
    vector<int> back;
    iota(idx.begin(), idx.end(), 0);

    vector<int> shuffle(54);
    for (int i = 0; i < 54; i++) cin >> shuffle[i];

    while (k--) {
        back = idx;
        for (int i = 0; i < 54; i++) {
            int j = shuffle[i] - 1;
            idx[j] = back[i];
        }
    }

    for (int i = 0; i < 54; i++) {
        cout << getCard(idx[i] + 1);
        if (i == 53) cout << '\n';
        else cout << ' ';
    }

    return 0;
}
```
