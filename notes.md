# C++
----
## 语法
----
### unordered_map

#### 1. 遍历
```c++
unordered_map<type1, type2> mp;
unordered_map<type1, type2>::iterator iter;
for (iter = mp.beging(); iter != mp.end(); iter++)
```
#### **unordered_map与map**
unordered_map: hash散列表
map: 红黑树
[两者的比较](https://blog.csdn.net/qq_21997625/article/details/84672775)
今天做到[一道题](https://leetcode-cn.com/problems/xor-queries-of-a-subarray/)的时候，领悟到了..     
用到了`unordered_map<pair<int, int>, int>`, 会报错
- unordered_map的键值只能是基本数据类型，否则需要自定义**哈希值求解**和**键值比较**的方法.
- 而map是基于红黑树的
最终改成了`map<pair<int, int>, int>`

### vector
#### 1. 初始化
```
vector<vector<int>> dp(n, vector<int> (m, INT_MAX))
```
#### 2. 常用函数
```c++
vector<int> nums;
nums.back();  // 取最后一个元素
nums.clear();   // 清除所有元素
reverse(nums.begin(), nums.end())   // 逆序
```

### string
#### 1. 类似于stack的方法
```c++
string s;
s.empty();   // stack.empty()
s.back();   // stack.top()
s.push_back();  //stack.push()
s.pop_back();   //stack.pop()

```
#### 2. 与数字字符串的转换
```c++
x = 10;
s = to_string(x); // "10"
```
### queue
#### 1. 常用函数
```c++
queue<int> q;
q.push(3);
q.front();
q.back();   // 返回最后一个元素(的引用)
q.pop();
q.empty();
q.size();
```
### priority_queue
使用基本数据类型时，默认是**大顶堆**。
```c++
priority_queue<int> q;
q.empty();
q.size();
q.top();
q.pop();
q.push(1116);
q.emplace(111); // 原地构造一个元素并push进队列
q.swap(1116, 111);
```
[优先队列使用](https://blog.csdn.net/weixin_36888577/article/details/79937886)
### deque(双端队列)
#### 1. 常用函数
```c++
deque<int> q;
q.empty();
q.size();
q.front();  <->    q.back();
q.push_front();   <->   q.push_back();
q.pop_front();  <->   q.pop_back();

```
## 奇淫巧函数
----
### 1.accumulate, 用于累加一个数组
```c++
accumulate(arr.begin(), arr.end(), addtition);
```
### 2. atoi, stoi
[atoi&stoi](https://blog.csdn.net/qq_33221533/article/details/82119031)
相同点：都是将数字字符串转换成int；头文件都是ctring；
不同点：atoi参数是const char*（如果是string的话需要用c_str转换），而stoi是const string*；atoi没有越界检查，越界了直接输出边界，而stoi越界了会runtime error

# Python
### list
#### 1. 声明指定长度的list  
https://www.jb51.net/article/142499.htm

# JAVA
---
## 语法
---
### 数组
#### 1. 声明与初始化
```java
int[] res = new int[n]; // 长度为n的integer型数组
Arrays.fill(res, -1);    // 每项初始赋值为-1
```
### stack
#### 1. 声明
方法一：元素类型是object。
```java
Stack stk = new Stack();
// 获取值时用：（int举例）
int top = Integer.parseInt(stk.peek().toString())   // 深井冰啊
```
方法二：类型自定义
```java
Stack <Integer> stack = new Stack < > ();
```
#### 2. 常用函数
```java
stack.push(object)
stack.empty() // bool
stack.peek()  // object 注意是object，即使push进去的是int之类的基础型变量
// 比如
stack.push(3)
stack.pop()   // pop()会返回栈顶元素，所以**取出栈顶元素并丢弃**的操作可以合二为一了。
```

### HashMap
#### 1. 声明
```java
HashMap <Integer, Integer> map = new HashMap < > ();
```

#### 2. 常用函数
赋值操作
```java
map.put(num1, num2); //map[num1] = num2
```
读取操作
```java
map.get(num1); //num2, 没有num1这个键值会报错
```