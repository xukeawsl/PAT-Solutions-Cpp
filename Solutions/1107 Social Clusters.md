注意点：

1. 并查集合并时如果两个集合已经在一起了，就不必再合并了，否则计数会有问题
2. 存在相同业余爱好的属于同一个集合，因此可以维护一个业余爱好到人的映射，只需要保存上一个人的 ID 即可
3. 输入存在一个 `:` ，可以用 `getchar()` 读掉

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <unordered_map>
#include <numeric>

using namespace std;

vector<int> ut;
vector<int> cnt;

int Find(int x) {
    if (x == ut[x]) return x;
    return ut[x] = Find(ut[x]);
}

void Union(int x1, int x2) {
    int root1 = Find(x1);
    int root2 = Find(x2);

    if (root1 == root2) return;

    ut[root1] = root2;
    cnt[root2] += cnt[root1];
    cnt[root1] = 0;
}

int main() {
    int n;
    cin >> n;

    ut.resize(n + 1);
    cnt.resize(n + 1, 1);

    iota(ut.begin(), ut.end(), 0);
    
    unordered_map<int, int> mp;
    
    for (int i = 1; i <= n; i++) {
        int c;
        cin >> c;
        getchar();
        for (int j = 0; j < c; j++) {
            int hobby;
            cin >> hobby;

            if (mp.count(hobby)) {
                Union(i, mp[hobby]);
            }
            
            mp[hobby] = i;
        }
    }

    vector<int> ans;
    
    for (int i = 1; i <= n; i++) {
        if (cnt[i]) ans.emplace_back(cnt[i]);
    }

    cout << ans.size() << endl;

    sort(ans.begin(), ans.end(), greater<int>());

    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i];
        if (i != ans.size() - 1) cout << ' ';
        else cout << '\n';
    }
    
    return 0;
}
```
