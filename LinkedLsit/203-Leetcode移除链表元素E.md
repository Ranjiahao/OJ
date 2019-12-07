<https://leetcode-cn.com/problems/remove-linked-list-elements/description/>

删除链表中等于给定值 **val** 的所有节点

**示例:**

```cpp
输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```



思路1：创建新链表，将不等于val的插入，时间O(n) 空间O(n)

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (!head) return nullptr; // 必须要有判空
        ListNode* newHead = nullptr;
        ListNode* last = newHead;
        ListNode* cur = head;
        while (cur) {
            if (cur->val != val) {
                ListNode* node = new ListNode(cur->val);
                if (newHead) {
                    last->next = node;
                    last = node;
                } else {
                    newHead = node;
                    last = newHead;
                }
            }
            cur = cur->next;
        }
        return newHead;
    }
};
```

思路2：先不管头节点，将后边删完，然后判断头节点， 时间O(n) 空间O(1)

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (!head) return nullptr;        
        ListNode* cur = head;
        while (cur->next) {
            if (cur->next->val == val) {
                ListNode* next = cur->next;
                cur->next = next->next;
                delete next;
            } else {
                cur = cur->next;
            }
        }
        //判断头
        if (head->val == val) {
                ListNode* ret = head->next;
                delete head;
                return ret;
        }
        return head;
    }
};
```

思路3：循环删除第一个节点，直到不等于val，判空，删除之后节点， 时间(n) 空间O(1)

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        //必须用循环解决，头几个节点可能都需要删除
        while(head && head->val == val) {
            ListNode* tmp = head;
            head = head->next;
            delete tmp;
        }
        if(head == nullptr) return head; // 这一步很重要，删头将链表全部删完了
        ListNode* cur = head;
        while(cur->next) {
            if(cur->next->val == val){
                ListNode* next = cur->next;
                cur->next = next->next;
                delete next;
            } else {
                cur = cur->next;
            }
        }
        return head;
    }
};
```

推荐思路4：假节点  时间O(n) 空间O(1) 

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        // 假节点
        ListNode* node = new ListNode(!val);
        node->next = head;
        
        ListNode* tmp = node;
        while (tmp->next) {
            if (tmp->next->val == val)
                tmp->next=tmp->next->next;
            else
                tmp=tmp->next;
        }
        head = node->next;
        delete node;
        return head;
    }
};
```

思路5：递归  时间O(n) 空间O(n) 

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (!head) return head;
        head->next = removeElements(head->next, val);
        return head->val == val ? head->next : head;
    }
};
```

