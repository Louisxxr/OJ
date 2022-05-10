根据 @北京大学郭炜 程序设计与算法(一)C语言程序设计课程整理。



# STL标准模板库

```c++
#include <iostream>
#include <algorithm>
#include <set>
#include <map>
using namespace std;
```



## 1) sort排序算法

### 从小到大排序

```c++
sort(数组名+n1, 数组名+n2);
```

将数组下标范围为[n1, n2)的元素从小到大排序（如果n1=0，则+n1可不写）

### 从大到小排序

```c++
sort(数组名+n1, 数组名+n2, greater<T>());
// T为元素类型
```

将数组下标范围为[n1, n2)的元素从大到小排序（如果n1=0，则+n1可不写）

### 按自定义规则排序

```c++
struct 结构名
{
    bool operator() (const T & a1, const T & a2) const {
        return ...;
    }
};
// T为元素类型
// 自定义结构名和T
sort(数组名+n1, 数组名+n2, 结构名());
```

```c++
// demo1
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

struct rule1 // 从大到小排序
{
    bool operator() (const int & a1, const int & a2) const {
        return a1 > a2;
    }
};

struct rule2 // 按个位数从小到大排序
{
    bool operator() (const int & a1, const int & a2) const {
        return a1%10 < a2%10;
    }
};

void print(int a[], int size) {
    for (int i=0; i<size; i++)
        cout << a[i] << ",";
    cout << endl;
}

int main()
{
    int a[] = {12, 45, 3, 98, 21, 7};
    sort(a, a+sizeof(a)/sizeof(int)); // 从小到大排序
    cout << "1) ";
    print(a, sizeof(a)/sizeof(int));
    sort(a, a+sizeof(a)/sizeof(int), rule1()); // 从大到小排序
    cout << "2) ";
    print(a, sizeof(a)/sizeof(int));
    sort(a, a+sizeof(a)/sizeof(int), rule2()); // 按个位数从小到大排序
    cout << "2) ";
    print(a, sizeof(a)/sizeof(int));
    return 0;
}

/*
输出结果为：
1) 3,7,12,21,45,98,
2) 98,45,21,12,7,3,
3) 21,12,3,45,7,98,
*/
```

```c++
// demo2
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

struct student
{
    char name[20];
    int id;
    double gpa;
};

student students[] = {{"Jack",112,3.4}, {"Mary",102,3.8}, {"Mary",117,3.9}, {"Ala",333,3.5}, {"Zero",101,4.0}};

struct rule1 // 按姓名从小到大排序
{
    bool operator() (const student & s1, const student & s2) const {
        if (stricmp(s1.name, s2.name) < 0)
            return true;
        return false;
    }
};

struct rule2 // 按id从小到大排序
{
    bool operator() (const student & s1, const student & s2) const {
        return s1.id < s2.id;
    }
};

struct rule3 // 按gpa从大到小排序
{
    bool operator() (const student & s1, const student & s2) const {
        return s1.gpa > s2.gpa;
    }
};
```



## 2) 二分查找算法

### binary_search

#### 对从小到大排好序的基本类型数组进行二分查找

```c++
binary_search(数组名+n1, 数组名+n2, 值);
```

#### 对按自定义规则排好序的基本类型数组进行二分查找

```c++
binary_search(数组名+n1, 数组名+n2, 值, 结构名());
```

[n1, n2)（如果n1=0，则+n1可不写）

查找成功，返回值为true(1)；否则，返回值为false(0)

“等于”的含义：“a必须在b前面”和“b必须在a前面”都不成立

```c++
// demo3
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

struct rule // 按个位数从小到大排序
{
    bool operator() (const int & a1, const int & a2) const {
        return a1%10 < a2%10;
    }
};

void print(int a[], int size) {
    for (int i=0; i<size; i++)
        cout << a[i] << ",";
    cout << endl;
}

int main()
{
    int a[] = {12, 45, 3, 98, 21, 7};
    sort(a, a+6);
    print(a, 6);
    cout << "result: " << binary_search(a, a+6, 12) << endl;
    cout << "result: " << binary_search(a, a+6, 77) << endl;
    sort(a, a+6, rule());
    print(a, 6);
    cout << "result: " << binary_search(a, a+6, 7) << endl;
    cout << "result: " << binary_search(a, a+6, 8, rule()) << endl;
    return 0;
}

/*
输出结果为：
3,7,12,21,45,98,
result: 1
result: 0
21,12,3,45,7,98,
result: 0          排序规则和查找规则不一致，故这个结果没有意义
result: 1          “等于”的含义：“a必须在b前面”和“b必须在a前面”都不成立
*/
```

### lower_bound

#### 对从小到大排好序的基本类型数组进行二分查找

```c++
lower_bound(数组名+n1, 数组名+n2, 值);
```

返回值为元素类型指针T* p：*p是查找区间里下标最小的、**大于等于“值”**的元素；如果找不到，p指向下标为n2的元素

#### 对按自定义规则排好序的基本类型数组进行二分查找

```c++
lower_bound(数组名+n1, 数组名+n2, 值, 结构名());
```

返回值为元素类型指针T* p：*p是查找区间里下标最小的、**按自定义规则可以排在“值”后面**的元素；如果找不到，p指向下标为n2的元素

### upper_bound

#### 对从小到大排好序的基本类型数组进行二分查找

```c++
upper_bound(数组名+n1, 数组名+n2, 值);
```

返回值为元素类型指针T* p：*p是查找区间里下标最小的、**大于“值”**的元素；如果找不到，p指向下标为n2的元素

#### 对按自定义规则排好序的基本类型数组进行二分查找

```c++
upper_bound(数组名+n1, 数组名+n2, 值, 结构名());
```

返回值为元素类型指针T* p：*p是查找区间里下标最小的、**按自定义规则必须排在“值”后面**的元素；如果找不到，p指向下标为n2的元素

```c++
// demo4
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

struct rule // 按个位数从小到大排序
{
    bool operator() (const int & a1, const int & a2) const {
        return a1%10 < a2%10;
    }
};

void print(int a[], int size) {
    for (int i=0; i<size; i++)
        cout << a[i] << ",";
    cout << endl;
}

int main()
{
    int a[7] = {12, 5, 3, 5, 98, 21, 7};
    
    sort(a, a+7);
    print(a, 7); // 3,5,5,7,12,21,98,
    int *p = lower_bound(a, a+7, 5);
    cout << *p << "," << p-a << endl; // 5,1
    p = upper_bound(a, a+7, 5);
    cout << *p << "," << p-a << endl; // 7,3
    
    sort(a, a+7, rule());
    print(a, 7); // 21,12,3,5,5,7,98,
    int *q = lower_bound(a, a+7, 16, rule());
    cout << *q << "," << q-a << endl; // 7,5
    if (upper_bound(a, a+7, 18, rule()) == a+7)
        cout << "not found" << endl;
    else
        cout << q-a << endl; // not found
    
    return 0;
}
```



## 3) 平衡二叉树数据结构

有时我们需要在增删数据的同时进行数据查找，希望增加数据、删除数据和查找数据都能在log(n)复杂度下完成

“容器” “迭代器”

### multiset

```c++
multiset<T> st;
```

这里定义一个multiset变量st，st里面存放数据类型为T的数据，并且能够自动排序

开始时st为空，可用st.insert添加元素，st.erase删除元素，st.find查找元素（复杂度都是log(n)）

排序规则：若表达式"a < b"为true，则a排在b前面

```c++
// demo5
#include <iostream>
#include <cstring>
#include <set> // 使用multiset和set需要此头文件
using namespace std;

int main()
{
    multiset<int> st;
    int a[10] = {1, 14, 12, 13, 7, 13, 21, 19, 8, 8};
    for (int i = 0; i < 10; i++)
        st.insert(a[i]);
    multiset<int>::iterator i;
    /*
    迭代器，相当于指针，指向multiset中的元素，访问multiset中的元素要通过迭代器
    multiset, set, multimap, map上的迭代器可++、--、用!=和==比较，但不可比大小、加减整数、相减（与指针的区别）
    st.begin()返回值类型为multiset<int>::iterator，是指向st的第一个元素的迭代器
    st.end()返回值类型为multiset<int>::iterator，是指向st的最后一个元素后面的迭代器
    对迭代器++，其指向下一个元素；对迭代器--，其指向上一个元素
    */
    for (i = st.begin(); i != st.end(); i++)
        cout << *i << ","; // 输出结果为：1, 7, 8, 8, 12, 13, 13, 14, 19, 21,
    cout << endl;
    
    i = st.find(22); // 若找到22，则返回指向该元素的迭代器；否则返回st.end()
    if (i == st.end())
        cout << "not found" << endl; // 输出结果为：not found
    st.insert(22);
    i = st.find(22);
    if (i == st.end())
        cout << "not found" << endl;
    else
        cout << "found:" << *i << endl; // 输出结果为：found:22
    i = st.lower_bound(13); // 返回迭代器p，使得[st.begin(), p)中的元素都在13前面
    cout << *i << endl; // 输出结果为：13
    i = st.upper_bound(8); // 返回迭代器p，使得[p, st.end())中的元素都在8后面
    cout << *i << endl; // 输出结果为：12
    st.erase(i); // 注意这里的参数为迭代器
    for (i = st.begin(); i != st.end(); i++)
        cout << *i << ","; // 输出结果为：1, 7, 8, 8, 13, 13, 14, 19, 21, 22,
    cout << endl;
    return 0;
}
```

```c++
// demo6
// 自定义排序规则的multiset用法
#include <iostream>
#include <cstring>
#include <set>
using namespace std;

struct rule // 按个位数从小到大排序
{
    bool operator() (const int & a1, const int & a2) const {
        return a1%10 < a2%10;
    }
};

int main()
{
    multiset<int, greater<int> > st; // 从大到小排序
    // ">>"报错：[Error] '>>' should be '> >' within a nested template argument list
    int a[10] = {1, 14, 12, 13, 7, 13, 21, 19, 8, 8};
    for (int i = 0; i < 10; i++)
        st.insert(a[i]);
    multiset<int, greater<int> >::iterator i;
    for (i = st.begin(); i != st.end(); i++)
        cout << *i << ","; // 输出结果为：21,19,14,13,13,12,8,8,7,1,
    cout << endl;
    
    multiset<int, rule> st2; // 按个位数从小到大排序
    for (int i = 0; i < 10; i++)
        st2.insert(a[i]);
    multiset<int, rule>::iterator p;
    for (p = st2.begin(); p != st2.end(); p++)
        cout << *p << ","; // 输出结果为：1,21,12,13,13,14,7,8,8,19,
    // 个位数相同、值不同的数的顺序不确定
    cout << endl;
    
    p = st2.find(133);
    cout << *p << endl; // 输出结果为：13
    // “等于”的含义：“a必须在b前面”和“b必须在a前面”都不成立
    return 0;
}
```

```c++
// demo7
// 自定义排序规则的multiset用法
#include <iostream>
#include <cstring>
#include <set>
using namespace std;

struct student
{
    char name[20];
    int id;
    int score;
};

student students[] = {{"Jack",112,78}, {"Mary",102,85}, {"Ala",333,92}, {"Zero",101,70}, {"Cindy",102,78}};

struct rule // 按分数从大到小排序；若分数相等，则按姓名从小到大排序
{
    bool operator() (const student & s1, const student & s2) const {
        if (s1.score != s2.score)
            return s1.score > s2.score;
        else
            return strcmp(s1.name, s2.name) < 0;
    }
};

int main()
{
    multiset<student, rule> st;
    for (int i = 0; i < 5; i++)
        st.insert(students[i]);
    multiset<student, rule>::iterator p;
    for (p = st.begin(); p != st.end(); p++)
        cout << p->score << " " << p->name << " " << p->id << endl;
    
    student s = {"Mary",1000,85};
    p = st.find(s);
    if (p != st.end())
        cout << p->score << " " << p->name << " " << p->id << endl;
    return 0;
}

/*
输出结果为：
92 Ala 333
85 Mary 102
78 Cindy 102
78 Jack 112
70 Zero 101
85 Mary 102
“等于”的含义：“a必须在b前面”和“b必须在a前面”都不成立
*/
```

### set

```c++
set<T> st;
```

set和multiset的区别在于容器里不能有相同元素（“等于”的含义：“a必须在b前面”和“b必须在a前面”都不成立）

所以，set添加元素可能不成功

```c++
// demo8
#include <iostream>
#include <cstring>
#include <set>
using namespace std;

int main()
{
    set<int> st;
    int a[10] = {1, 2, 3, 8, 7, 7, 5, 6, 8, 12};
    for (int i = 0; i < 10; i++)
        st.insert(a[i]);
    cout << st.size() << endl; // 输出结果为：8
    set<int>::iterator i;
    for (i = st.begin(); i != st.end(); i++)
        cout << *i << ","; // 输出结果为：1,2,3,5,6,7,8,12,
    cout << endl;
    
    pair<set<int>::iterator, bool> result = st.insert(2);
    /*
    pair<T1, T2>数据类型等价于：
    struct {
    	T1 first;
    	T2 second;
    };
    */
    if (! result.second) // 条件成立说明插入不成功
        cout << *result.first << " already exists." << endl; // 若插入不成功，则返回指向容器中已存在的相同元素的迭代器
    else
        cout << *result.first << " inserted." << endl; // 若插入成功，则返回指向该元素的迭代器
    // 输出结果为：2 already exists.
    return 0;
}
```

### multimap

```c++
multimap<T1, T2> mp;
// mp里的元素都是如下类型：
struct {
    T1 first; // 关键字
    T2 second; // 值
};
```

multimap里的元素按照first从小到大或自定义规则排序，按照first查找

##### 应用实例

一个学生成绩录入和查询系统，接受以下两种输入：
Add name id score
Query score

name是个不超过16字符的字符串，中间没有空格，代表学生姓名；id是个整数，代表学号；score是个整数，表示分数。学号不会重复，分数和姓名都可能重复。

两种输入交替出现。第一种输入表示要添加一个学生的信息，碰到这种输入，就记下学生的姓名、id和分数；第二种输入表示要查询，碰到这种输入，就输出已有记录中分数比score低的最高分获得者的姓名、学号和分数。如果有多个学生都满足条件，就输出学号最大的那个学生的信息。如果找不到满足条件的学生，则输出"Nobody"。

输入样例：
Add Jack 12 78
Query 78
Query 81
Add Percy 9 81
Add Marry 8 81
Query 82
Add Tom 11 79
Query 80
Query 81

输出样例：
Nobody
Jack 12 78
Percy 9 81
Tom 11 79
Tom 11 79

```c++
// demo9
#include <iostream>
#include <cstring>
#include <map> // 使用multimap和map需要此头文件
using namespace std;

struct StudentInfo
{
    int id;
    char name[20];
};

struct Student
{
    int score;
    StudentInfo info;
};

typedef multimap<int, StudentInfo> MAP_STD;

int main()
{
    MAP_STD mp;
    Student st;
    char cmd[20];
    while (cin >> cmd) {
        if (cmd[0] == 'A') {
            cin >> st.info.name >> st.info.id >> st.score;
            mp.insert(make_pair(st.score, st.info));
        } // make_pair类似函数，生成一个pair<int, StudentInfo>变量，其first等于st.score，second等于st.info
        else if(cmd[0] == 'Q') {
            int score;
            cin >> score;
            MAP_STD::iterator p = mp.lower_bound(score);
            if (p != mp.begin()) {
                p--;
                score = p->first; // 为什么是->first而不是->score，思考“容器”和“迭代器”的关系
                MAP_STD::iterator maxp = p;
                int maxid = p->second.id;
                for (; p != mp.begin() && p->first == score; p--) {
                    if (p -> second.id > maxid) {
                        maxp = p;
                        maxid = p->second.id;
                    }
                }
                // 如果上面循环是因为p == mp.begin()而终止，则p指向的元素还要处理
                if (p->first == score) {
                    if (p->second.id > maxid) {
                        maxp = p;
                        maxid = p->second.id;
                    }
                }
                cout << maxp->second.name << " "
                     << maxp->second.id << " "
                     << maxp->first << endl;
            }
            else
                cout << "Nobody" << endl; // 如果lower_bound的结果就是mp.begin()，说明没人分数比查询分数低
        }
    }
    return 0;
}
```

### map

map和multimap的区别在于容器里不能有关键字重复的元素（“等于”的含义：“a必须在b前面”和“b必须在a前面”都不成立）

所以，map添加元素可能不成功

可以使用[ ]，下标为关键字，返回值为first和关键字相同的元素的second

```c++
// demo10
#include <iostream>
#include <map>
#include <string>
using namespace std;

struct Student
{
    string name;
    int score;
};

Student students[5] = {{"Jack",89}, {"Tom",74}, {"Cindy",87}, {"Alysa",87}, {"Micheal",98}};

typedef map<string, int> MP;

int main()
{
    MP mp;
    for (int i = 0; i < 5; i++)
        mp.insert(make_pair(students[i].name, students[i].score));
    cout << mp["Jack"] << endl; // 输出结果为：89
    mp["Jack"] = 60;
    for (MP::iterator i = mp.begin(); i != mp.end(); i++)
        cout << "(" << i->first << "," << i->second << ") "; // 输出结果为：(Alysa,87) (Cindy,87) (Jack,60) (Micheal,98) (Tom,74) 
    cout << endl;
    Student st;
    st.name = "Jack";
    st.score = 99;
    pair<MP::iterator, bool> p = mp.insert(make_pair(st.name, st.score));
    if (p.second)
        cout << "(" << p.first->first << "," << p.first->second << ") inserted" << endl;
    else
        cout << "insertion failed" << endl;
    // 输出结果为：insertion failed
    mp["Harry"] = 78; // 插入一元素，其first为"Harry"，然后将其second改为78
    MP::iterator q = mp.find("Harry");
    cout << "(" << q->first << "," << q->second << ")" << endl; // 输出结果为：(Harry,78)
    return 0;
}
```

##### 应用实例

输入大量单词，每个单词一行，不超过20字符，没有空格。按出现次数从多到少输出这些单词及其出现次数。出现次数相同的，字典序靠前的在前面。

输入样例：
this
is
ok
this
plus
that
is
plus
plus

输出样例：
plus 3
is 2
this 2
ok 1
that 1

```c++
// demo11
#include <iostream>
#include <set>
#include <map>
#include <string>
using namespace std;

struct Word
{
    int times;
    string wd;
};

struct Rule
{
    bool operator() (const Word & w1, const Word & w2) const {
        if (w1.times != w2.times)
            return w1.times > w2.times;
        else
            return w1.wd < w2.wd; // 有#include <string>的头文件，可以直接比较
    }
};

int main()
{
    string s;
    set<Word, Rule> st;
    map<string, int> mp;
    while (cin >> s)
        mp[s]++; // 当map元素值为int类型或者常量时候，默认值为0
    for (map<string, int>::iterator i = mp.begin(); i != mp.end(); i++) {
        Word tmp;
        tmp.wd = i->first;
        tmp.times = i->second;
        st.insert(tmp);
    }
    for (set<Word, Rule>::iterator i = st.begin(); i != st.end(); i++)
        cout << i->wd << " " << i->times << endl;
    return 0;
}
```