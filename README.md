# PAT 甲级题解

记录 PAT 甲级题目的 C++ 解答及其注意点, 并分享一些 C++ 刷题常用语法

试题链接：

1. [PAT官方](https://pintia.cn/problem-sets/994805342720868352/exam/problems/type/7)

2. [AcWing](https://www.acwing.com/problem/search/1/?csrfmiddlewaretoken=BZzs1DHQHamtbsPlyW88aT2Bt7nCuCta08kfejLs7EMgLEx7UozjHDTjCOiw3u82&show_algorithm_tags=0&search_content=PAT)

# C++ 刷题常用语法

## 离散化（数组去重）

```cpp
#include <vector>
#include <algorithm>

using namespace std;

vector<int> arr = {1, 3, 2, 2, 4, 5};

sort(arr.begin(), arr.end());	// O(nlogn)

auto last = unique(arr.begin(), arr.end());  // 将相邻元素去重, last 为去重后的尾迭代器
arr.erase(last, arr.end());	// 删除 [last, arr.end()) 这部分重复出现的元素

// 以上步骤可以简化为如下, 时间复杂度是 O(n) 的
// arr.erase(unique(arr.begin(), arr.end()), arr.end());
```

## 数组求和

```cpp
#include <vector>
#include <numeric>

using namespace std;

vector<int> arr = {1, 3, 2, 2, 4, 5};

int sum = accumulate(arr.begin(), arr.end(), 0);
```

## 求数组最值

```cpp
#include <vector>
#include <algorithm>

using namespace std;

vector<int> arr = {1, 3, 2, 2, 4, 5};

int maxval = *max_element(arr.begin(), arr.end());  // 返回最大值元素对应的迭代器
int minval = *min_element(arr.begin(), arr.end());  // 返回最小值元素对应的迭代器

// 返回最小值和最大值对应的迭代器(pair)
auto [minit, maxit] = minmax_element(arr.begin(), arr.end());
```

## 构建前缀和数组

```cpp
#include <vector>
#include <numeric>

using namespace std;

vector<int> arr = {1, 3, 2, 2, 4, 5};

vector<int> presum(arr.size() + 1);	// 需要分配足够的空间
partial_sum(arr.begin(), arr.end(), presum.begin() + 1);
```

## 读取输入行（包括空格）

```cpp
#include <iostream>
#include <string>

using namespace std;

string input;
getline(cin, input);	// 如果之前有非字符串类型读取, 需要用 getchar() 读走回车符
```

## 根据空格分隔多个字符串

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>

using namespace std;

string input = "hello world c++";
    
istringstream in(input);

vector<string> words;
string word;
while (in >> word) {
    words.emplace_back(word);
}
```

## 优先级队列自定义比较规则

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

priority_queue<int> pq_max;    // 默认是最大堆

priority_queue<int, vector<int>, greater<int>> pq_min; // 最小堆

using PII = pair<int, int>;

// 自定义比较规则, first 大的在上, first 相同时, second 小的在上
// priority_queue 的比较函数含义同 sort 相反, p1.first < p2.first 表示
// first 更大的在上, 而 sort 中表示 first 更小的在前
auto cmp = [](const PII& p1, const PII& p2) {
    if (p1.first == p2.first) {
        return p1.second > p2.second;
    }
    return p1.first < p2.first;
};

priority_queue<PII, vector<PII>, decltype(cmp)> pq_self{cmp};

pq_self.emplace(2, 1);
pq_self.emplace(2, 3);
pq_self.emplace(1, 3);

while (!pq_self.empty()) {
    auto [a, b] = pq_self.top(); pq_self.pop();
    cout << a << '-' << b << endl;
}
```

## 数组翻转

```cpp
#include <vector>
#include <algorithm>

using namespace std;

vector<int> arr = {1, 3, 2, 2, 4, 5};

reverse(arr.begin(), arr.end());
```

## 求下一个排列

```cpp
#include <string>
#include <algorithm>

using namespace std;

string s = "123";

// 下一个更大的排列是 132
// 如果不存在下一个更大的排列则返回 false, 且 s 转换为最小的排列
bool exist = next_permutation(s.begin(), s.end());
```

## 初始化一个下标数组

```cpp
#include <vector>
#include <numeric>

using namespace std;

vector<int> index(5);

// 从 0 开始填充, {0, 1, 2, 3, 4}
iota(index.begin(), index.end(), 0);
```

## 字符串数字互转

```cpp
#include <string>

using namespace std;

string s = "666";

int digit = stoi(s);    // 字符串转数字
//long digit = stol(s);
//long long digit = stoll(s);

s = to_string(digit + 1);   // 数字转字符串
```

## 十进制转二进制字符串

```cpp
#include <iostream>
#include <string>
#include <bitset>

using namespace std;

bitset<32> b(-234);	// 指定 32 位整数

cout << b.to_string() << endl;	// 对于负数, 得到的是补码的字符串表示

cout << b.count() << endl; // 得到二进制表示下 1 的个数
```

## 最大公约数/最小公倍数

```cpp
#include <algorithm>

using namespace std;

int a = 3, b = 6;

int g = __gcd(a, b);	// algorithm 提供了 __gcd 但是没有提供 lcm

// 自己实现 lcm
int lcm(int a, int b) {
    return a * b / __gcd(a, b);
}


// c++17 标准 numeric 提供了 gcd 和 lcm 函数
// 不过它们适用于 common_type，对于 __int128_t 这种类型不适用
// 可以使用上面的 __gcd 来实现
#inlucde <numeric>

int a = 3, b = 4;

int g = gcd(a, b);
int l = lcm(a, b);
```

## 红黑树容器使用

### 迭代器

```cpp
// 注意点 1, 关于指向末尾的迭代器
// set 和 map 的尾迭代器 end 存放了容器中的元素个数
// 因此解引用 end 迭代器是没问题的, 对 end 执行自减操作
// 可以得到容器尾部的元素
auto it = prev(st.end());

// 注意点 2，关于指向起始的迭代器
// set 和 map 的起始迭代器当容器为空时和 end 相同
// 对 begin 执行自减操作还是得到的 begin 而不是 end
// 因此需要注意不要造成死循环了
```

### 自定义比较规则

```cpp
// 以 pair 为例, 假设规定以 first 从大到小排列, first 相同时
// 以 second 从小到大排列

using PII = pair<int, int>;

auto cmp = [](const PII& p1, const PII& p2) {
    if (p1.first == p2.first) {
        return p1.second < p2.second;
    }
    return p2.first < p1.first;
};

set<PII, decltype(cmp)> st(cmp);
```
