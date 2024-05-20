注意点：

1. 词频统计，数字也属于单词的一部分，单词通过非数字、字母符号分隔，注意最后读取完需要检查最后一个单词是否为空
2. 最大词频对应多个单词时，按字典序最小返回

```cpp
#include <iostream>
#include <string>
#include <map>

using namespace std;

int main() {
    string speech;
    getline(cin, speech);

    map<string, int> cnt;

    string word;
    for (auto ch : speech) {
        if (!isalnum(ch)) {
            if (word.size()) {
                cnt[word]++;
                word.clear();
            }
        } else {
            if (isdigit(ch)) word.push_back(ch);
            else word.push_back(tolower(ch));
        }
    }

    if (word.size()) cnt[word]++;

    string ans;
    int maxcnt = 0;
    for (auto& [w, c] : cnt) {
        if (c > maxcnt) {
            maxcnt = c;
            ans = w;
        }
    }

    cout << ans << ' ' << maxcnt << endl;
    

    return 0;
}
```
