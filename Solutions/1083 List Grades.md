注意点：

1. 简单排序即可，没有满足条件的时输出 `NONE` 

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>

using namespace std;

struct Stu {
    string name;
    string id;
    int grade;
};

int main() {
    int n;
    cin >> n;

    vector<Stu> s(n);
    for (int i = 0; i < n; i++) {
        cin >> s[i].name >> s[i].id >> s[i].grade;
    }
    int grade1, grade2;
    cin >> grade1 >> grade2;

    sort(s.begin(), s.end(), [](const Stu& s1, const Stu& s2){
        return s1.grade > s2.grade;
    });

    int cnt = 0;
    for (auto& stu : s) {
        if (stu.grade >= grade1 && stu.grade <= grade2) {
            cout << stu.name << ' ' << stu.id << endl;
            cnt++;
        }
    }

    if (cnt == 0) {
        cout << "NONE" << endl;
    }
    
    return 0;
}
```
