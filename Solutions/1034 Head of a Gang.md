注意点：

1. 一个帮派要超过 2 个人，也就是连通分量的大小需要大于 2
2. 连通分量中每个人通话的总时间大于阈值才算满足条件，由于每条边关联两个点，因此可以每个点都加上时间，最后除以二就是总时间
3. 输入边数最大是 1000，因此最多有 2000 个不同的人，注意设置的数组大小

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <unordered_map>

using namespace std;

int id_gen;
unordered_map<string, int> id;
unordered_map<int, int> total;

vector<string> name(2001);
vector<bool> vis(2001);
vector<vector<int>> graph(2001);

int dfs(int u, int& head, int& sum) {
    vis[u] = true;
    if (head == -1 || total[u] > total[head]) {
        head = u;
    }

    sum += total[u];

    int cnt = 1;

    for (auto v : graph[u]) {
        if (vis[v]) continue;
        cnt += dfs(v, head, sum);
    }

    return cnt;
}

int main() {
    int n, k;

    cin >> n >> k;

    for (int i = 0; i < n; i++) {
        string name1, name2;
        int time;
        cin >> name1 >> name2 >> time;
        if (!id.count(name1)) {
            id[name1] = id_gen++;
        }

        if (!id.count(name2)) {
            id[name2] = id_gen++;
        }

        int u = id[name1], v = id[name2];
            
        total[u] += time;
        total[v] += time;
    
        name[u] = name1;
        name[v] = name2;
        
        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    map<string, int> mp;
    
    for (int i = 0; i < id_gen; i++) {
        if (vis[i]) continue;
        int cnt, head = -1;
        int sum = 0;
        cnt = dfs(i, head, sum);
        sum /= 2;
        if (cnt > 2 && sum > k) {
            mp[name[head]] = cnt;
        }
    }

    cout << mp.size() << endl;
    for (auto& [h, c] : mp) {
        cout << h << ' ' << c << endl;
    }
    
    return 0;
}
```
