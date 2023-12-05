---
title: 大三寒假学C++&lc
date:  2022-02-22 13:21:01 
tags:
- C++
- leetcode
- 算法
- 一个月
categories: 
- 笔记
---

### C++

### 大三上暑假苦苦学c++肝lc，记录一下每天的笔记

<!-- more -->

### 1.1

> 两数之和(1) count = 1;

c++ map

``` c++
第一种：用insert函数插入pair数据：
map<int,string> my_map;
my_map.insert(pair<int,string>(1,"first"));
my_map.insert(pair<int,string>(2,"second"));

第二种：用insert函数插入value_type数据：
map<int,string> my_map;
my_map.insert(map<int,string>::value_type(1,"first"));
my_map.insert(map<int,string>::value_type(2,"second"));

map<int,string>::iterator it;           //迭代器遍历
for(it=my_map.begin();it!=my_map.end();it++)
    cout<<it->first<<it->second<<endl;

第三种：用数组的方式直接赋值：
map<int,string> my_map;
my_map[1]="first";
my_map[2]="second";

map<int,string>::iterator it;
for(it=my_map.begin();it!=my_map.end();it++)
    cout<<it->first<<it->second<<endl;

```

### 1.2

``` c++
#include <algorithm>
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        map<int,int> m1;
        map<int,int> m2;
        vector<int> a;
        for(int i = 0; i < nums1.size(); i++){
            m1[nums1[i]]++;
        }
        for(int i = 0; i < nums2.size(); i++){
            m2[nums2[i]]++;
        }
        map<int, int>::iterator iter;
          for (iter = m1.begin(); iter != m1.end(); iter++) {
            int minT = min<int>(m2[iter->first],iter -> second);
            if( minT> 0){
                for(int j = 0; j < minT; j++){
                    a.push_back(iter -> first);
                }         
            }
        }
        return a;
    }
};
```

### 1.3

> 最大子数组和 = 2
>
> 用两个栈实现队列 = 1	



``` c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max = nums[0];
        int lSize = nums.size();
        for(int i = 1; i < lSize; i++){
            if(nums[i - 1] > 0){
                nums[i] += nums[i - 1];
            }
            if(nums[i] > max){
                max = nums[i];
            }
        }
        return max;
    }
};
```

``` c++
class CQueue {
    stack<int> s1,s2;
public:
    CQueue() {
        
    }
    
    void appendTail(int value) {
        s1.push(value);
    }
    
    int deleteHead() {
        if(!s2.empty())
        {
            int a=s2.top();
            s2.pop();
            return a;
        }
        if(s1.empty())  return -1;
        while(!s1.empty())
          {
                s2.push(s1.top());
                s1.pop();
                
          }
          int b = s2.top();
          s2.pop();
          return b;
    }
};
```

Stack操作

```c++
stack<int> q;	//以int型为例
int x;
q.push(x);		//将x压入栈顶
q.top();		//返回栈顶的元素
q.pop();		//删除栈顶的元素
q.size();		//返回栈中元素的个数
q.empty();		//检查栈是否为空,若为空返回true,否则返回false
```

### 1.4

```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head == nullptr) return nullptr;
        Node* cur = head;
        unordered_map<Node*, Node*> map;
        // 3. 复制各节点，并建立 “原节点 -> 新节点” 的 Map 映射
        while(cur != nullptr) {
            map[cur] = new Node(cur->val);
            cur = cur->next;
        }
        cur = head;
        // 4. 构建新链表的 next 和 random 指向
        while(cur != nullptr) {
            map[cur]->next = map[cur->next];
            map[cur]->random = map[cur->random];
            cur = cur->next;
        }
        // 5. 返回新链表的头节点
        return map[head];
    }
};

```

### 1.16

queue

```c++
push() 在队尾插入一个元素
pop() 删除队列第一个元素
size() 返回队列中元素个数
empty() 如果队列空则返回true
front() 返回队列中的第一个元素
back() 返回队列中最后一个元素
```

### 1.17

二叉树镜像 辅助栈做法未学

### 1.18

斐波那契数列的矩阵幂算法

```c++
class Solution {
public:
    const int MOD = 1000000007;

    int fib(int n) {
        if (n < 2) {
            return n;
        }
        vector<vector<long>> q{{1, 1}, {1, 0}};
        vector<vector<long>> res = pow(q, n - 1);
        return res[0][0];
    }

    vector<vector<long>> pow(vector<vector<long>>& a, int n) {
        vector<vector<long>> ret{{1, 0}, {0, 1}};
        while (n > 0) {
            if (n & 1) {
                ret = multiply(ret, a);
            }
            n >>= 1;
            a = multiply(a, a);
        }
        return ret;
    }

    vector<vector<long>> multiply(vector<vector<long>>& a, vector<vector<long>>& b) {
        vector<vector<long>> c{{0, 0}, {0, 0}};
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                c[i][j] = (a[i][0] * b[0][j] + a[i][1] * b[1][j]) % MOD;
            }
        }
        return c;
    }
};

```

### 1.20

循环条件有可能不经意间 因为变量改变 ——寻找bug

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *res = new ListNode(0), *p = res;
        while(l1 && l2 ){
            l1->val < l2->val ? (p->next = l1,l1 = l1->next) : (p->next = l2, l2 = l2->next);
            p = p->next;              
        }
        p->next = l1 ? l1 : l2;
        return res->next;
    }
};
```

### 1.23

判断条件最好不要用相加后的结果，应该用target - nums[i] 跟 nums[j]比较，这样保证不会溢出。

同样的例子还有二分查找，(left + right) / 2 可以用left + ((rigth - left) >> 1))代替

### 1.26

回溯时

``` c++
path.pop_back();
```

功力

### 1.28

```c++
void quickSort(vector<int>& arr, int l, int r){
    if(l >= r) return;
    int i = l, j = r;
    while(i < j){
      while(i < j && arr[j] >= arr[l]){j--;};
      while(i < j && arr[i] <= arr[l]){i++;};
      swap(arr[i], arr[j]);
    }
    swap(arr[i], arr[l]);
    quickSort(arr, l, i - 1);
    quickSort(arr, i + 1, r);
}
```

