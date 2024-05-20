注意点：

1. 题目说每对玩家最多 2 小时，需要考虑输入大于 2 小时的情况
2. 营业时间是 08:00:00 ~ 21:00:00 不包括边界，因此如果玩家选择球台的结束时间大于等于 21:00:00 时，将不再处理
3. 对于一对新来的玩家，选择可用球台中编号最小的，但是如果当前玩家是 vip 并且可用球台中存在 vip 球台，则选择 vip 球台中编号最小的，如果没有可用球台，则选择结束时间最早且编号最小的球台，如果当前是 vip 用户则尝试选择 vip 球台
4. 最后结果按照开始服务时间排序，如果存在相同的服务时间则按照到达时间排序
5. 输出的等待时间四舍五入为整数分钟

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <cstdio>
#include <map>
#include <cmath>

using namespace std;

struct Player {
    string arrive;
    int time;
    int vip;
    string serve;
    int wait;
};

int tosec(const string& t) {
    int h, m, s;
    sscanf(t.c_str(), "%d:%d:%d", &h, &m, &s);
    return h * 60 * 60 + m * 60 + s;
}

string totime(int total) {
    int h = total / 3600;
    int m = (total - h * 3600) / 60;
    int s = total - h * 3600 - m * 60;

    char tmp[20] = {0};
    sprintf(tmp, "%02d:%02d:%02d", h, m, s);

    return tmp;
}

int main() {
    int n, k, m;
    cin >> n;

    vector<Player> players(n);

    for (int i = 0; i < n; i++) {
        cin >> players[i].arrive >> players[i].time >> players[i].vip;
        players[i].time = min(players[i].time, 120);
        players[i].time *= 60;
        players[i].wait = 0;
    }

    sort(players.begin(), players.end(), [](const Player& p1, const Player& p2){
        return p1.arrive < p2.arrive;
    });

    vector<bool> vis(n);
    map<string, int> mp;
    for (int i = 0; i < n; i++) {
        if (players[i].vip) {
            mp[players[i].arrive] = i;
        }
    }

    cin >> k >> m;

    vector<int> tables(k + 1);
    vector<int> base(k + 1, tosec("08:00:00"));
    vector<int> cnt(k + 1);
    vector<bool> isvip(k + 1);

    for (int i = 0; i < m; i++) {
        int number;
        cin >> number;
        isvip[number] = true;
    }

    for (int i = 0; i < players.size();) {
        if (vis[i]) {
            i++;
            continue;
        }
        
        auto& p = players[i];
        
        
        if (p.arrive >= "21:00:00") {
            break;
        }

        // find smallest number
        int s = 0;
        for (int j = 1; j <= k; j++) {
            if (tosec(p.arrive) >= base[j]) {
                if (s == 0) {
                    s = j;
                }
                if (p.vip && isvip[j]) {
                    s = j;
                    break;
                }
            }
        }

        if (s == 0) {
            int mins = 1e9;
            for (int j = 1; j <= k; j++) {
                if (base[j] < mins) {
                    mins = base[j];
                    s = j;
                }
            }
            
            for (int j = 1; j <= k; j++) {
                if (base[j] == mins && p.vip && isvip[j]) {
                    s = j;
                    break;
                }
            }
        }

        if (base[s] >= tosec("21:00:00")) {
            break;
        }
        
        cnt[s]++;
        
        // vip privilege 
        if (isvip[s] && !mp.empty() && tosec(mp.begin()->first) <= base[s]) {
            int idx = mp.begin()->second;
            mp.erase(mp.begin());
            vis[idx] = true;
            
            players[idx].serve = totime(base[s]);
            players[idx].wait += base[s] - tosec(players[idx].arrive);
            base[s] += players[idx].time;

            if (players[idx].arrive == p.arrive) {
                i++;
            }

        } else {
            if (p.vip) {
                mp.erase(mp.begin());    
            }
            
            if (tosec(p.arrive) <= base[s]) {
                p.serve = totime(base[s]);
                p.wait += base[s] - tosec(p.arrive);
                base[s] += p.time;
            } else {
                p.serve = p.arrive;
                base[s] = tosec(p.arrive) + p.time;
            }
        
            vis[i] = true;
            i++;
        }
    }

    sort(players.begin(), players.end(), [](const Player& p1, const Player& p2){
        if (p1.serve == p2.serve) {
            return p1.arrive < p2.arrive;
        }
       return p1.serve < p2.serve; 
    });

    for (auto p : players) {
        if (p.serve.empty()) continue;
        int minu = p.wait / 60;
        minu += round((p.wait - minu * 60) / 60.0);
        cout << p.arrive << ' ' << p.serve << ' ' << minu << endl;
    }

    for (int i = 1; i <= k; i++) {
        cout << cnt[i];
        if (i == k) cout << '\n';
        else cout << ' ';
    }
    
    return 0;
}
```
