注意点：

1. 选择的 pivot 需要满足条件：比左边的元素都大，比右边的元素都小

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> arr(n), lmax(n);
    int l = -1;
    
    for (int i = 0; i < n; i++) {
        lmax[i] = l;
        
        cin >> arr[i];
        l = max(l, arr[i]);
    }

    vector<int> ans;

    int r = 1e9;
    for (int i = n - 1; i >= 0; i--) {
        if (lmax[i] < arr[i] && r > arr[i]) {
            ans.emplace_back(arr[i]);
        }
        r = min(r, arr[i]);
    }

    sort(ans.begin(), ans.end());

    cout << ans.size() << endl;

    if (ans.empty()) cout << '\n';
    
    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i];
        if (i != ans.size() - 1) cout << ' ';
        else cout << '\n';
    }
    
    return 0;
}
```
