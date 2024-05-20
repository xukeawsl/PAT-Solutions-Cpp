注意点：

1. 除了动态求中位数外还需要支持 Pop 操作，因此不能使用堆，可以使用红黑树，支持动态删除，思路和对顶堆求中位数相同，如果有 k 个数，k 为偶数时，两个 set 大小相同，k 为奇数时，st1 数量多一个
2. 插入元素时要判断一下是否大于 st2 中的最小值，是则替换，不是则插入 st1 中，每轮都对两个 set 进行大小调整
3. 大小调整时，不能使用 st1.size() - st2.size() > 1 的条件，因为 size() 方法返回的无符号整数，当 st1 数量小于 st2 时会出问题，应该改成 st1.size() > st2.size() + 1
4. 为了精确删除栈顶元素，可以为元素绑定一个唯一标识，这样不会在两个 set 中都存在相同值时重复删除，也可以使用 multiset，但是当从一个 set 中删除了元素后不能再去删除另一个 set 的元素了

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <set>

using namespace std;

using PII = pair<int, int>;

int main() {
    int n;
    cin >> n;

    stack<PII> stk;
    set<PII, greater<PII>> st1;
    set<PII> st2;

    for (int i = 0; i < n; i++) {
        string cmd;
        int value;
        cin >> cmd;
        
        if (cmd == "Pop") {
            if (stk.empty()) {
                cout << "Invalid" << endl;
            } else {
                auto p = stk.top(); stk.pop();
                cout << p.first << endl;
                auto it = st1.find(p);
                if (it != st1.end() && *it == p) {
                    st1.erase(it);
                }
                it = st2.find(p);
                if (it != st2.end() && *it == p) {
                    st2.erase(it);
                }
            }
        } else if (cmd == "PeekMedian") {
            if (st1.empty()) {
                cout << "Invalid" << endl;
            } else {
                cout << st1.begin()->first << endl;
            }
        } else {
            cin >> value;
            stk.emplace(value, i);
            if (!st2.empty() && value > st2.begin()->first) {
                auto it = st2.begin();
                st1.emplace(*it);
                st2.erase(it);
                st2.emplace(value, i);
            } else {
                st1.emplace(value, i);
            }
        }
        
        while (!st1.empty() && st1.size() > st2.size() + 1) {
            auto it = st1.begin();
            st2.emplace(*it);
            st1.erase(it);
        }

        while (!st2.empty() && st2.size() > st1.size()) {
            auto it = st2.begin();
            st1.emplace(*it);
            st2.erase(it);
        }
    }
    
    return 0;
}
```
