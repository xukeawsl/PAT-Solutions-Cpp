注意点：

1. map 加 set 轻松统计

```cpp
#include <iostream>
#include <string>
#include <map>
#include <set>

using namespace std;

int main() {
    int n, k;
    cin >> n >> k;

    map<string, set<int>> coures;

    for (int i = 0; i < k; i++) {
        int id, m;
        cin >> id >> m;
        for (int j = 0; j < m; j++) {
            string name;
            cin >> name;
            coures[name].insert(id);
        }
    }

    for (int i = 0; i < n; i++) {
        string query;
        cin >> query;
        cout << query << ' ' << coures[query].size();
        for (int id : coures[query]) {
            cout << ' ' << id;
        }
        cout << endl;
    }
    
    return 0;
}
```
