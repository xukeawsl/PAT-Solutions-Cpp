注意点：

1. 图书 id 虽说是 7 位数字，但是不能够按整数读取，因为可能有前导 0，也算 id 的一部分
2. 在读取关键字时可以使用 <sstream> 库中的 istringstream 来实现分割读取
3. 在读取行之前，如果有读取整数的操作，为了避免读取到回车，要先用 getchar 吃回车，然后使用 getline(cin, xx) 来读取整行字符串，默认是读取到回车，回车会丢弃，与 fgets 区分

```cpp
#include <iostream>
#include <unordered_set>
#include <string>
#include <algorithm>
#include <sstream>

using namespace std;

struct book {
    string id;
    string title;
    string author;
    unordered_set<string> key_words;
    string publisher;
    string publisher_year;
};

int main() {
    int n, m;
    cin >> n;
    getchar();

    vector<book> bs(n);

    for (int i = 0; i < n; i++) {
        getline(cin, bs[i].id);
        getline(cin, bs[i].title);
        getline(cin, bs[i].author);
        string words;
        getline(cin, words);
        istringstream ss(words);
        while (ss >> words) {
            bs[i].key_words.insert(words);
        }
        getline(cin, bs[i].publisher);
        getline(cin, bs[i].publisher_year);
    }

    sort(bs.begin(), bs.end(), [](const book& b1, const book& b2) {
        return b1.id < b2.id;
    });

    cin >> m;
    getchar();
    
    string query;
    for (int i = 0; i < m; i++) {
        getline(cin, query);
        cout << query << endl;
        string str = query.substr(3);

        bool flag = false;
        
        if (query[0] == '1') {
            for (auto& b : bs) {
                if (b.title == str) {
                    flag = true;
                    cout << b.id << endl;
                }
            }
        }

        if (query[0] == '2') {
            for (auto& b : bs) {
                if (b.author == str) {
                    flag = true;
                    cout << b.id << endl;
                }
            }
        }

        if (query[0] == '3') {
            for (auto& b : bs) {
                if (b.key_words.count(str)) {
                    flag = true;
                    cout << b.id << endl;
                }
            }
        }

        if (query[0] == '4') {
            for (auto& b : bs) {
                if (b.publisher == str) {
                    flag = true;
                    cout << b.id << endl;
                }
            }
        }

        if (query[0] == '5') {
            for (auto& b : bs) {
                if (b.publisher_year == str) {
                    flag = true;
                    cout << b.id << endl;
                }
            }
        }

        if (!flag) cout << "Not Found" << endl;
    }
    
    return 0;
}
```

