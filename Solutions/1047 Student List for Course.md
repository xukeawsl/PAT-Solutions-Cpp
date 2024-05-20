注意点：

1. 本题会卡输入输出和哈希表的时间，需要用 vector 数组模拟哈希，最后再排序输出
2. 输出的时候注意是需要输出所有课程的注册情况，没有学生注册的也要打印

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <cstdio>

using namespace std;

int main() {
    int n, k;
    cin >> n >> k;
    
    vector<vector<string>> stu(n + 1);

    for (int i = 0; i < n; i++) {
        char name[10];
        int cnt, id;
        scanf("%s %d\n", name, &cnt);
        for (int j = 0; j < cnt; j++) {
            int course;
            scanf("%d", &course);
            stu[course].push_back(name);
        }
    }

    for (int i = 1; i <= k; i++) {
        cout << i << ' ' << stu[i].size() << endl;
        sort(stu[i].begin(), stu[i].end());
        for (auto& name : stu[i]) {
            printf("%s\n", name.c_str());
        }
    }
    
    return 0;
}
```
