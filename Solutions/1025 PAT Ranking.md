注意点：

1. 相同分数的拥有相同排名，假如前面 k 个成绩相同，则第 k + 1 的排名是 k + 1 而不是 2

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

struct Data {
    string id;
    int score;
    int lnumber;
    int frank;
    int lrank;
};

int main() {
    int n, k;
    cin >> n;

    vector<Data> vec;
    
    for (int i = 1; i <= n; i++) {
        cin >> k;
        Data d;
        d.lnumber = i;
        while (k--) {
            cin >> d.id >> d.score;
            vec.push_back(d);
        }
    }

    sort(vec.begin(), vec.end(), [&](const Data& a, const Data& b) {
        if (a.score == b.score) {
            return a.id < b.id;
        }
        return a.score > b.score;
    });

    cout << vec.size() << endl;

    int local_rank[301] = {0};
    int local_pres[301] = {0};
    int local_idx[301] = {0};
    int final_rank = 0;

    int prescore = 0, idx = 0;
    for (auto& d : vec) {
        if (final_rank == 0) {
            prescore = d.score;
            d.frank = ++final_rank;
        } else {
            if (d.score == prescore) {
                d.frank = final_rank;
            } else {
                prescore = d.score;
                d.frank = idx + 1;
                final_rank = idx + 1;
            }
        }
        ++idx;

        if (local_rank[d.lnumber] == 0) {
            local_pres[d.lnumber] = d.score;
            d.lrank = ++local_rank[d.lnumber];
        } else {
            if (d.score == local_pres[d.lnumber]) {
                d.lrank = local_rank[d.lnumber];
            } else {
                local_pres[d.lnumber] = d.score;
                d.lrank = local_idx[d.lnumber] + 1;
                local_rank[d.lnumber] = local_idx[d.lnumber] + 1;
            }
        }
        ++local_idx[d.lnumber];

        cout << d.id << ' ' << d.frank << ' ' << d.lnumber << ' ' << d.lrank << endl;
    }

    return 0;
}


```
