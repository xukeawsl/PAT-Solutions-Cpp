注意点：

1. 成绩排序不能采用字符串形式，因为长度不一致，例如 31 小于 132，但是按字典序 132 更小

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <tuple>

using namespace std;

struct Stu {
    string id;
    string name;
    int grade;
};

int main() {
    int n, c;
    cin >> n >> c;

    vector<Stu> records(n);

    for (int i = 0; i < n; i++) {
        cin >> records[i].id >> records[i].name >> records[i].grade;
    }

    sort(records.begin(), records.end(), 
         [&](const Stu& a, const Stu& b) {
        if (c == 1) {
            return a.id < b.id;
        }

        if (c == 2) {
            if (a.name == b.name) {
                return a.id < b.id;
            }
            return a.name < b.name;
        }

        if (c == 3) {
            if (a.grade == b.grade) {
                return a.id < b.id;
            }
            return a.grade < b.grade;
        }  
    });

    for (auto& record : records) {
        cout << record.id << ' ' << record.name << ' ' << record.grade << endl;
    }
    
    return 0;
}
```
