注意点：

1. 考虑到数量，直接全部读取后再排序，对于每次查询都遍历一下排序后的结果，将满足条件的打印出来
2. AcWing 会卡超时，不能将结果存放到 vector 中会超时，且读取和输入使用 C 语言的

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

struct people {
    string name;
    int age;
    int worth;
};

int main() {
    int n, k;
    scanf("%d %d", &n, &k);

    vector<people> p(n);
    for (int i = 0; i < n; i++) {
        char name[10];
        int a, w;
        scanf("%s %d %d", name, &a, &w);
        p[i].name = name;
        p[i].age = a;
        p[i].worth = w;
    }

    sort(p.begin(), p.end(), [](const people& p1, const people& p2) {
        if (p1.worth == p2.worth && p1.age == p2.age) {
            return p1.name < p2.name;
        } else if (p1.worth == p2.worth) {
            return p1.age < p2.age;
        }
        return p2.worth < p1.worth;
    });

    for (int j = 0; j < k; j++) {
        int mcnt, l, r;
        scanf("%d %d %d", &mcnt, &l, &r);
        printf("Case #%d:\n", j + 1);
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            if (cnt == mcnt) {
                break;
            }
            if (p[i].age >= l && p[i].age <= r) {
                printf("%s %d %d\n", p[i].name.c_str(), p[i].age, p[i].worth);
                cnt++;
            }
        }
        if (cnt == 0) puts("None");
    }
    
    return 0;
}
```
