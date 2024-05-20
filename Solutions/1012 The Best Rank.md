注意点：

1. 平均成绩从示例来看是四舍五入取整数的，可以通过 round 函数来计算，注意参数要转为浮点数
2. 当某几科的 rank 相同时，优先级为 A > C > M > E，因此排序按照上面的顺序进行，在比较时只有后面的严格大于前面的 rank 时才替换
3. 排序时为了方便可以使用索引数组，需要引入头文件 <numeric>，使用 iota 来初始化 index 数组
4. 对于某一个成绩来说，相同成绩的学生 rank 是相同的，而且假如 3 个人 rank 都是 1，那么下个 rank 是从 4 开始的，而不是 2（坑点）

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <unordered_map>
#include <string>
#include <numeric>
#include <cmath>

using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    unordered_map<string, pair<int, char>> grade;

    vector<string> stuid;
    vector<int> vc, vm, ve, va;
    
    for (int i = 0; i < n; i++) {
        string id;
        int c, m, e;
        cin >> id >> c >> m >> e;
        stuid.push_back(id);
        vc.push_back(c);
        vm.push_back(m);
        ve.push_back(e);
        va.push_back(round((c + m + e) / 3.0));
    }

    auto func = [&](vector<int>& v, char ch) {
        vector<int> index(n);
        iota(index.begin(), index.end(), 0);
    
        sort(index.begin(), index.end(), [&](int pa, int pb) {
            return v[pa] > v[pb];
        });
    
        int rank = 0;
        int pre = -1;
        for (int i = 0; i < index.size(); i++) {
            int idx = index[i];
            if (v[idx] != pre) {
                rank = i + 1;
            }
            pre = v[idx];
            string id = stuid[idx];
            if (grade.count(id)) {
                if (grade[id].first > rank) {
                    grade[id] = {rank, ch};
                }
            } else {
                grade[id] = {rank, ch};
            }
        }
    };

    func(va, 'A');
    func(vc, 'C');
    func(vm, 'M');
    func(ve, 'E');

    while (m--) {
        string id;
        cin >> id;
        if (grade.count(id)) {
            cout << grade[id].first << ' ' << grade[id].second << endl;
        } else {
            cout << "N/A" << endl;
        }
    }

    return 0;
}
```
