
# 数据结构原理
1. 数组的地址是连续的。所以通过[index]索引就可以访问数组中任意一个元素的值。因为数组名就是数组首元素的地址，指针+i，就是跨过了。
# 代码模版
```cpp
/******************************************************************************

                              Online C++ Compiler.
               Code, Compile, Run and Debug C++ program online.
Write your code in this editor and press "Run" button to compile and execute it.

*******************************************************************************/

#include <iostream>
#include <vector>
using namespace std;
// 链表节点定义
struct node{
  int val;
  node* next;
  // 构造函数，无参，1个参数，2个参数
  node(): val(0), next(nullptr){}
  node(int val): val(val), next(nullptr){}
  node(int val, node* next): val(val), next(next){}
};
// 打印链表
void print_linked_list(node* head){
    cout << "--------start print---------" << endl;
    node* cur = head;
    while(cur != nullptr){
        cout << cur->val << endl;
        cur = cur->next;
    }
    cout << "--------print finished.--------" << endl;
}
int main()
{
    /*
    // 任务1：构造链表：先定义node，然后链接node构成链表
    node* head = new node();
    node* node1 = new node();
    node* node2 = new node();
    node* node3 = new node();
    node* node4 = new node();
    node* node5 = new node();
    head->next = node1;  // 头节点的值无意义，指向第一个有值的节点node1
    node1->val = 1;
    node1->next = node2;
    node2->val = 2;
    node2->next = node3;
    node3->val = 3;
    node3->next = node4;
    node4->val = 4;
    node4->next = node5;
    node5->val = 5;
    node5->next = nullptr;
    
    print_linked_list(head);
    */
    
    // 任务2：构造链表：给定一个数组，构造链表
    vector<int> arr = {6,7,8,9,10};
    node* head = new node();
    // 一个指针也可以完成 数组->链表的转化。
    node* cur = new node();
    head->next = cur;
    for(int i = 0; i < arr.size(); i++){
        cur->val = arr[i];
        // 本来cur->next初始值为nullptr，这里是让它指向一个有意义的地址
        // new node()得到这个地址对应的节点，val和next都是默认值。
        cur->next = new node();  
        cur = cur->next;
    }
    print_linked_list(head);
    
    // 任务3：增加：在第i个节点后增加一个节点
    cur = head;  // cur初始化
    int add_index = 3;
    int add_value = 1024;
    // cur从head开始遍历，找到第i个节点
    for(int i = 0; i < add_index; i++){
        cur = cur->next;
    }
    node* add_node = new node(add_value);
    // 链表中新增一个节点需要3步：1个保存，2个链接。
    node* post = cur->next;  // 保存cur->next的地址
    cur->next = add_node;  // cur链接新添加的node
    add_node->next = post;  // 新node链接 cur之前的next节点
    
    print_linked_list(head);
    
    // 任务4：删除：删除第i个节点
    cur = head;  // 游标初始化
    int del_index = 3;
    // del_index-1是因为删除第i个节点，要在前一个节点开始删除，所以找到第i-1个节点
    for(int i = 0; i < del_index-1; i++){
        cur = cur->next;
    }
    // 删除节点就一行代码：跨过next，链接next->next。
    cur->next = cur->next->next;
    print_linked_list(head);
    
    // 任务5：删除：删除值为value的节点
    // 这个任务分为2部分: (1)找到值为value的节点 (2)删除节点 
    int del_value = 7;
    cur = head;  
    // 通过前一个节点来判断val，所以是cur->next->val
    while(cur->next != nullptr){
        if(cur->next->val == del_value){
            break;
        }
        cur = cur->next;
    }
    cur->next = cur->next->next;
    print_linked_list(head);
    
    // 任务5：修改：修改第i个节点的值
    // 分为两步：(1)先查找 (2)修改值
    cur = head;
    int find_index = 3;
    int new_value = 2048;
    // 因为cur是从head开始的，所以走一步cur->next才到了第一个节点。
    for(int i = 0; i < find_index; i++){
        cur = cur->next;
    }
    cur->val = new_value;
    cout << "第" << find_index << "个节点的值是：" << cur->val << endl;
    print_linked_list(head);
    
    // 任务6：查找：查找第i个节点（同任务5）
    
    // 任务7：查找：查找值为val的节点的index
    cur = head;
    int find_value = 10;
    int res_index = 0;
    while(cur != nullptr){
        if(cur->val == find_value){
            break;
        }
        res_index++;
        cur = cur->next;
    }
    cout << "值为" << find_value << "的节点的index是：" << res_index << endl;

    return 0;
}
```
# 题目分类
