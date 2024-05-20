注意点：

1. 所给的头指针可能表示空指针，这时应该输出 "0 -1"
2. 不能直接对所有节点进行排序，需要根据所给头指针遍历整条链表，实际需要排序的节点数可能少于给定节点数
3. AcWing 卡 string 哈希表，可以用 int 来存放地址值，不过输出的时候注意格式

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <unordered_map>

using namespace std;

int main() {
    int n;
    int head;

    cin >> n >> head;

    unordered_map<int, pair<int, int>> mp;
    
    for (int i = 0; i < n; i++) {
        int addr, next;
        int key;
        cin >> addr >> key >> next;
        mp[addr] = {key, next};
    }

    vector<pair<int, int>> lst;

    if (head == -1) {
        cout << "0 -1" << endl;
        return 0;
    }

    while (head != -1) {
        lst.emplace_back(mp[head].first, head);
        head = mp[head].second;
    }

    sort(lst.begin(), lst.end());

    printf("%d %05d\n", lst.size(), lst[0].second);
    
    for (int i = 0; i < lst.size(); i++) {
        if (i == lst.size() - 1) {
            printf("%05d %d -1\n", lst[i].second, lst[i].first);
        } else {
            printf("%05d %d %05d\n", lst[i].second, lst[i].first, lst[i + 1].second);
        }
    }
    
    return 0;
}
```
