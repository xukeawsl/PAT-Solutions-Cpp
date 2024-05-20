注意点：

1. 家庭成员构成无向图，注意人员 ID 可能有 0 的

```cpp
#include <iostream>
#include <cstdio>
#include <vector>
#include <unordered_map>
#include <cstring>
#include <algorithm>

using namespace std;

vector<vector<int>> graph(10000);
vector<bool> vis(10000);
unordered_map<int, pair<double, double>> mp;

struct Res {
    int ID;
    int M;
    double avg_sets;
    double avg_area;
};

void dfs(int u, Res& r) {
    r.avg_sets += mp[u].first;
    r.avg_area += mp[u].second;
    r.M++;
    r.ID = min(r.ID, u);
    
    for (int v : graph[u]) {
        if (!vis[v]) {
            vis[v] = true;
            dfs(v, r);
        }
    }
}

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; i++) {
        int id, father, mother;
        cin >> id >> father >> mother;
        if (father != -1) {
            graph[father].emplace_back(id);
            graph[id].emplace_back(father);
        }
        if (mother != -1) {
            graph[mother].emplace_back(id);
            graph[id].emplace_back(mother);
        }
        int k;
        cin >> k;
        for (int j = 0; j < k; j++) {
            int child;
            cin >> child;
            graph[id].emplace_back(child);
            graph[child].emplace_back(id);
        }
        double m, a;
        cin >> m >> a;
        mp[id] = {m, a};
    }

    vector<Res> ans;
    
    for (int i = 0; i <= 10000; i++) {
        if (mp.count(i) && !vis[i]) {
            vis[i] = true;
            Res r;
            memset(&r, 0, sizeof(Res));
            r.ID = 10000;
            dfs(i, r);
            r.avg_area /= r.M;
            r.avg_sets /= r.M;
            ans.emplace_back(r);
        }
    }

    sort(ans.begin(), ans.end(), [](const Res& r1, const Res& r2) {
        if (r1.avg_area == r2.avg_area) {
            return r1.ID < r2.ID;
        }
        return r2.avg_area < r1.avg_area;
    });

    cout << ans.size() << endl;
    for (int i = 0; i < ans.size(); i++) {
        printf("%04d %d %.3lf %.3lf\n", ans[i].ID, ans[i].M, ans[i].avg_sets, ans[i].avg_area);
    }
    
    return 0;
}
```
