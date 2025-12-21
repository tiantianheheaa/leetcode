
# 数据结构原理
1. 数组的地址是连续的。所以通过[index]索引就可以访问数组中任意一个元素的值。因为**数组名就是数组首元素的地址**，指针+i，就是跨过了i*(数据类型字节数)，所以直接指向的是下一个元素的首地址。
2. 链表的地址是不连续的。所以除了value字段，还有一个next指针，指向下一个元素的地址。
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
## 基本操作
### 203-移除链表元素
1. 总结：一个比较完善的删除链表元素的代码。 考虑了各种情况，head为空节点，删除head元素，多个节点的值为val，多个连续节点的值为val等等。
2. 代码思路：就是在cur->next = cur->next->next; 的基础上完善了各种情况的处理。
3. 虚拟头结点：dummy_head的好处。可以置身事外，处理head为空，删除head等特殊情况。**【有百利而无一害】**，初始化是dummy->next=head，返回结果是dummy->next。没有任何影响。
4. 信念：**【追求真理，遇水搭桥，逢山开路】**。这里题目有4种写法，是否使用dummy_head，删除节点用单指针/双指针，叉乘后是4种写法。写法不同决定了每个写法都会遇到不同的代码问题，秉持着**遇到问题-明确问题-解决问题**的思路，遇到问题后case by case的分析和解决，朝着正确的方向，就一定能走到最后。 **而不是死记硬背4种写法的代码**。每一遍刷题，都站在不同的高度。
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        // 【生产级代码的标准】一定要注意边界条件、特殊case的处理。
        if(head == nullptr){
            return head;
        }
        // dummy_head的优点：置身事外，可以删除head本身。
        ListNode* dummy_head = new ListNode();
        dummy_head->next = head;
        // cur游标，从dummy_head开始，通过判断cur->next->val 站在前面一步判断，方便用一个指针删除节点。
        ListNode* cur = new ListNode();
        cur = dummy_head;
        while(cur->next != nullptr){
            if(cur->next->val == val){
                // 当下一个节点被删除，就相当于cur向前走了一步。因为cur->next已经变了。
                cur->next = cur->next->next;
            }else{
                // 不要把cur=cur->next放else外面，否则if走完，还要走cur=cur->next，就是走两步了。 但是cur->next!=nullptr需要每走一步都判断一次
                cur = cur->next;
            }
        }
        // 循环推出是因为cur->next == nullptr，也就是cur指向最后一个节点。 并且由于是在前一个位置判断，即cur->next->val 是否等于val。所以cur当前指向的节点，已经在前一步判断过其值了。
        // 因为head节点本身可能被删除，所以返回值是dummy_head->next
        return dummy_head->next;
    }
};
// 移除所有值为val的元素
// 头节点的好处： 因为可以移除第一个元素。
// 题目中给的链表都不是 虚拟头节点。 
// 设置虚拟头节点，因为有可能删除的是原链表的第一个节点。
// 删除节点的可以用单个指针，cur->next = cur->next->next。 也可以用2个指针。

// 【追求真理，相信真理，而不是死记硬背套路】 不论怎么写，dummy_head是否存在；用cur一个指针，还是cur和pre两个指针。叉乘后有4种写法。不论哪种写法都可以通过，但是每种写法遇到的问题不同，处理方法不同。不能死记硬背4种写法，但是只要会debug，【遇到问题-明确问题-解决问题】，就一定能达到最终的结果。【只要有一颗追求真理的心，遇水搭桥，逢山开路，就一定能走到最后。】
```
## 链表反转

## 其他题目
