注意点：

1. 不要把不存在的情况当成成绩为 0 来看，直接按 -1 来算，从公式可以发现，当期末为 -1 期中不为 -1 时，一定不会入选

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

struct Stu {
    string id;
    int Gp;
    int Gmid;
    int Gfinal;
    int G;
};

unordered_map<string, Stu> mp;

int main() {
    int p, m, n;
    string id;
    int score;
    cin >> p >> m >> n;
    for (int i = 0; i < p; i++) {
        cin >> id >> score;
        if (score < 200) continue;
        Stu s;
        s.id = id;
        s.Gp = score;
        s.Gmid = s.Gfinal = -1;
        mp[id] = s;
    }

    for (int i = 0; i < m; i++) {
        cin >> id >> score;
        if (!mp.count(id)) continue;
        mp[id].Gmid = score;
    }

    for (int i = 0; i < n; i++) {
        cin >> id >> score;
        if (!mp.count(id)) continue;
        mp[id].Gfinal = score;
    }

    vector<Stu> vec;

    for (auto& [_, s] : mp) {
        if (s.Gmid > s.Gfinal) {
            s.G = round((s.Gmid * 0.4 + s.Gfinal * 0.6));
        } else {
            s.G = s.Gfinal;
        }
        if (s.G >= 60 && s.G <= 100) vec.emplace_back(s);
    }

    sort(vec.begin(), vec.end(), [](const Stu& s1, const Stu& s2) {
        if (s1.G == s2.G) {
            return s1.id < s2.id;
        }
        return s2.G < s1.G;
    });

    for (auto& s : vec) {
        cout << s.id << ' ' << s.Gp << ' ' << s.Gmid << ' ' << s.Gfinal << ' ' << s.G << endl;
    }
    
    return 0;
}
```
