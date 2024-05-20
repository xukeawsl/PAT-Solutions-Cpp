注意点：

1. 排名按照总成绩来，总成绩相同时按照国家入学考试等级来排，如果还是相同，那么他们的排名是相同的
2. 每个人有多个选择，从左到右依次尝试，如果都不行就不录取
3. 如果相同排名的人都预期进入同一个研究生院，那么可以忽略人数限制，前提是这个研究生院当前还有名额

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int n, m, k;
vector<int> limits;
vector<vector<int>> graduates;

struct Stu {
    int rank;
    int idx;
    int ge, gi;
    int choices[5];
};

int canIn(const Stu& s) {
    for (int i = 0; i < k; i++) {
        int t = s.choices[i];
        if (graduates[t].size() < limits[t]) {
            return t;
        }
    }
    return -1;
}

int main() {
    cin >> n >> m >> k;

    limits.resize(m);
    graduates.resize(m);
    
    for (int i = 0; i < m; i++) cin >> limits[i];

    vector<Stu> s(n);
    
    for (int i = 0; i < n; i++) {
        cin >> s[i].ge >> s[i].gi;
        for (int j = 0; j < k; j++) {
            cin >> s[i].choices[j];
        }
        s[i].idx = i;
    }

    sort(s.begin(), s.end(), [](const Stu& s1, const Stu& s2){
        if (s1.ge + s1.gi == s2.ge + s2.gi) {
            return s1.ge > s2.ge;
        }
        return s1.ge + s1.gi > s2.ge + s2.gi;
    });

    for (int i = 0, rank = 0; i < n; i++) {
        if (i > 0 && s[i].ge == s[i - 1].ge && s[i].gi == s[i - 1].gi) {
            s[i].rank = rank;
        } else {
            rank = i + 1;
            s[i].rank = rank;
        }
    }

    vector<bool> st(n);
    for (int i = 0; i < n; i++) {
        if (st[i]) continue;
        int t = canIn(s[i]);
        if (t == -1) continue;
        vector<int> add;
        st[i] = true;
        add.emplace_back(s[i].idx);
        int j = i + 1;
        while (j < n && s[j].rank == s[i].rank) {
            if (!st[j] && canIn(s[j]) == t) {
                st[j] = true;
                add.emplace_back(s[j].idx);
            }
            j++;
        }

        for (auto x : add) graduates[t].emplace_back(x);
    }

    for (int i = 0; i < m; i++) {
        sort(graduates[i].begin(), graduates[i].end());
        if (graduates[i].empty()) {
            cout << '\n';
        } else {
            for (int j = 0; j < graduates[i].size(); j++) {
                cout << graduates[i][j];
                if (j == graduates[i].size() - 1) cout << '\n';
                else cout << ' ';
            }
        }
    }
    
    return 0;
}
```
