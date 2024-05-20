注意点：

1. 所有题目都是未回答或编译失败的人不算在排名中
2. 打印结果时，对于没有回答的问题打印 `-`，对于编译失败的题目打印 `0`

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <cstring>

using namespace std;

const int INF = 0xcfcfcfcf;

int scores[100001][6];
int p[6];

struct Stu {
    int id;
    int score[6];
    int total;
    int cnt_perfect;
};

int main() {
    int n, k, m;

    scanf("%d %d %d", &n, &k, &m);

    memset(scores, 0xcf, sizeof(scores));

    for (int i = 1; i <= k; i++) scanf("%d", &p[i]);

    for (int i = 0; i < m; i++) {
        int id, pro, score;
        scanf("%d %d %d", &id, &pro, &score);
        scores[id][pro] = max(scores[id][pro], score);
    }

    vector<Stu> stu;
    
    for (int id = 1; id < 100000; id++) {
        Stu s;
        s.id = id;
        s.total = 0;
        s.cnt_perfect = 0;
        bool flag = false;
        for (int i = 1; i <= k; i++) {
            if (scores[id][i] != INF && scores[id][i] != -1) {
                flag = true;
                s.total += scores[id][i];
                if (scores[id][i] == p[i]) {
                    s.cnt_perfect++;
                }
            }
            s.score[i] = scores[id][i];
        }
        if (flag) stu.emplace_back(s);
    }

    sort(stu.begin(), stu.end(), [](const Stu& s1, const Stu& s2){
        if (s1.total == s2.total) {
            if (s1.cnt_perfect == s2.cnt_perfect) {
                return s1.id < s2.id;
            }
            return s1.cnt_perfect > s2.cnt_perfect;
        }
        return s1.total > s2.total;
    });

    for (int i = 0, rank = 1; i < stu.size(); i++) {
        if (i > 0 && stu[i].total != stu[i - 1].total) {
            rank = i + 1;
        }
        printf("%d %05d %d", rank, stu[i].id, stu[i].total);
        for (int j = 1; j <= k; j++) {
            if (stu[i].score[j] == -1) {
                printf(" 0");
            } else if (stu[i].score[j] == INF) {
                printf(" -");
            } else printf(" %d", stu[i].score[j]);
        }
        putchar('\n');
    }
    
    return 0;
}
```
