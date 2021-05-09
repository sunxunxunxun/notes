# C++
----
## 语法
----
### map
#### 1. 遍历
```
map<type1, type2> mp;
map<type1, type2>::iterator iter;
for (iter = mp.beging(); iter != mp.end(); iter++)
```
### vector
#### 1. 初始化
```
vector<vector<int>> dp(n, vector<int> (m, INT_MAX))
```
#### 2. 常用函数
```
vector<int> nums;
nums.back();  //取最后一个元素
nums.clear();   //清除所有元素
```
### string
#### 1. 类似于stack的方法
```
string s;
s.empty();   // stack.empty()
s.back();   // stack.top()
s.push_back();  //stack.push()
s.pop_back();   //stack.pop()

```
#### 2. 与数字字符串的转换
```
x = 10;
s = to_string(x); // "10"
```
### queue
#### 1. 常用函数
```
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
```
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
```
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
```
accumulate(arr.begin(), arr.end(), addtition);
```
### 2. atoi, stoi
[atoi&stoi](https://blog.csdn.net/qq_33221533/article/details/82119031)
相同点：都是将数字字符串转换成int；头文件都是ctring；
不同点：atoi参数是const char*（如果是string的话需要用c_str转换），而stoi是const string*；atoi没有越界检查，越界了直接输出边界，而stoi越界了会runtime error


# JAVA
---
## 语法
---
### 数组
#### 1. 声明与初始化
```
int[] res = new int[n]; // 长度为n的integer型数组
Arrays.fill(res, -1);    // 每项初始赋值为-1
```
### stack
#### 1. 声明
方法一：元素类型是object。
```
Stack stk = new Stack();
// 获取值时用：（int举例）
int top = Integer.parseInt(stk.peek().toString())   // 深井冰啊
```
方法二：类型自定义
```
Stack <Integer> stack = new Stack < > ();
```
#### 2. 常用函数
```
stack.push(object)
stack.empty() // bool
stack.peek()  // object 注意是object，即使push进去的是int之类的基础型变量
// 比如
stack.push(3)
stack.pop()   // pop()会返回栈顶元素，所以**取出栈顶元素并丢弃**的操作可以合二为一了。
```

### HashMap
#### 1. 声明
```
HashMap <Integer, Integer> map = new HashMap < > ();
```

#### 2. 常用函数
赋值操作
```
map.put(num1, num2); //map[num1] = num2
```
读取操作
```
mag.get(num1); //num2, 没有num1这个键值会报错
```