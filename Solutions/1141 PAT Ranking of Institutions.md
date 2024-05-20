注意点：

1. 根据题目统计后排序即可

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <algorithm>
#include <vector>

using namespace std;

struct Score {
    int B;
    int A;
    int T;
    
    Score() : B(0), A(0), T(0) {}
};

struct Data {
    string school;
    int TWS;
    int Ns;
};

void stolow(string& s) {
    for (auto& ch : s) {
        ch = tolower(ch);
    }
}

int main() {
    int n;
    cin >> n;

    unordered_map<string, Score> mp;
    unordered_map<string, int> cnt;

    for (int i = 0; i < n; i++) {
        string id, school;
        int score;
        cin >> id >> score >> school;
        stolow(school);
        cnt[school]++;
        if (id[0] == 'B') {
            mp[school].B += score;
        } else if (id[0] == 'A') {
            mp[school].A += score;
        } else {
            mp[school].T += score;
        }
    }

    vector<Data> vec;
    for (auto& [sch, s] : mp) {
        Data d;
        d.school = sch;
        d.TWS = s.B / 1.5 + s.A + s.T * 1.5;
        d.Ns = cnt[sch];
        vec.emplace_back(d);
    }

    sort(vec.begin(), vec.end(), [](const Data& d1, const Data& d2) {
        if (d1.TWS == d2.TWS) {
            if (d1.Ns == d2.Ns) {
                return d1.school < d2.school;
            }
            return d1.Ns < d2.Ns;
        }
        return d2.TWS < d1.TWS;
    });

    cout << vec.size() << endl;

    for (int i = 0, rank = 1; i < vec.size(); i++) {
        if (i > 0 && vec[i].TWS != vec[i - 1].TWS) {
            rank = i + 1;
        }
        cout << rank << ' ' << vec[i].school << ' ' << vec[i].TWS << ' ' << vec[i].Ns << endl;
    }

    return 0;
}
```
