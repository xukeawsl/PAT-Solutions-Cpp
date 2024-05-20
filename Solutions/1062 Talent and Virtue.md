注意点：

1. 对于不同类型人根据类型排序，划分类型时需要注意 below 和 no more than 等代表的含义，有没有包括等于的情况
2. 最后一行默认不包含回车

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

struct people {
    string id;
    int virtue;
    int talent;
};

int n, l, h;

int type(const people& p) {
    if (p.virtue >= h && p.talent >= h) return 1;
    if (p.virtue >= h && p.talent < h) return 2;
    if (p.virtue < h && p.talent < h && p.virtue >= p.talent) return 3;
    return 4;
}

int total(const people& p) {
    return p.virtue + p.talent;
}

bool same(const people& p1, const people& p2) {
    return p1.virtue == p2.virtue && p1.talent == p2.talent;
}

int main() {
    cin >> n >> l >> h;

    vector<people> vec;
    for (int i = 0; i < n; i++) {
        people p;
        cin >> p.id >> p.virtue >> p.talent;
        if (p.virtue < l || p.talent < l) {
            continue;
        }
        vec.push_back(p);
    }

    sort(vec.begin(), vec.end(), [&](const people& p1, const people& p2) {
        if (type(p1) != type(p2)) return type(p1) < type(p2);
        if (total(p1) == total(p2)) {
            if (same(p1, p2)) {
                return p1.id < p2.id;
            }
            return p1.virtue > p2.virtue;
        }
        return total(p1) > total(p2);
    });

    cout << vec.size() << endl;

    for (int i = 0; i < vec.size(); i++) {
        cout << vec[i].id << ' ' << vec[i].virtue << ' ' << vec[i].talent;
        if (i != vec.size() - 1) cout << '\n';
    }
    
    return 0;
}
```
