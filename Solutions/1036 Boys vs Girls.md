注意点：

1. 遍历取最大最小值即可

```cpp
#include <iostream>
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

    vector<Stu> male, female;

    for (int i = 0; i < n; i++) {
        Stu s;
        char gender;
        cin >> s.name >> gender >> s.id >> s.grade;
        if (gender == 'M') {
            male.push_back(s);
        } else {
            female.push_back(s);
        }
    }

    int fp = -1, highest = -1;
    int mp = -1, lowest = 1e9;

    for (int i = 0; i < female.size(); i++) {
        if (female[i].grade > highest) {
            highest = female[i].grade;
            fp = i;
        }
    }

    for (int i = 0; i < male.size(); i++) {
        if (male[i].grade < lowest) {
            lowest = male[i].grade;
            mp = i;
        }
    }

    if (fp == -1) cout << "Absent" << endl;
    else cout << female[fp].name << ' ' << female[fp].id << endl;

    if (mp == -1) cout << "Absent" << endl;
    else cout << male[mp].name << ' ' << male[mp].id << endl;

    if (fp == -1 || mp == -1) cout << "NA" << endl;
    else cout << female[fp].grade - male[mp].grade << endl;
    
    return 0;
}
```
