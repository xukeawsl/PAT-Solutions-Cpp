注意点：

1. 每组的人数是 n / k，如果不能整除则多余的人放在最后一组
2. 输出时是倒序的

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <deque>

using namespace std;

struct People {
    int height;
    string name;
};

int main() {
    int n, k;
    cin >> n >> k;

    vector<People> p(n);
    
    for (int i = 0; i < n; i++) {
        cin >> p[i].name >> p[i].height;
    }

    sort(p.begin(), p.end(), [](const People& p1, const People& p2) {
        if (p1.height == p2.height) {
            return p2.name < p1.name;
        }
        return p1.height < p2.height;
    });

    int group_cnt = n / k;

    vector<deque<string>> ans;
    
    for (int c = 0; c < k; c++) {
        deque<string> dq;
        int i = c * group_cnt;
        int j;
        if (c == k - 1) {
            j = n - 1;
        } else {
            j = i + group_cnt - 1;
        }
        dq.push_back(p[j].name);
        for (int k = j - 1; k >= i; k--) {
            if (dq.size() % 2) dq.push_front(p[k].name);
            else dq.push_back(p[k].name);
        }
        ans.emplace_back(move(dq));
    }

    reverse(ans.begin(), ans.end());

    for (auto& dq : ans) {
        for (int k = 0; k < dq.size(); k++) {
            cout << dq[k];
            if (k != dq.size() - 1) cout << ' ';
            else cout << '\n';
        }
    }
    
    return 0;
}
```
