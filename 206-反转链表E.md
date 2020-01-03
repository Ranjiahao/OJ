<https://leetcode-cn.com/problems/reverse-linked-list/description/>

反转一个单链表。

**示例:**

```cpp
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```



思路1：新建链表，不断头插即可， 时间O(n) 空间O(n)

```cpp
class Solution {
    public:
    ListNode* reverseList(ListNode* head) {
        ListNode* result = NULL;
        ListNode* cur = head;
        while (cur != NULL) {
            ListNode* next = cur->next;
            // 头插并更新头节点
            cur->next = result;
            result = cur; 
            cur = next;
        }
        return result;
    }
};
```

思路2：递归，画图找关系即可， 时间O(n) 空间O(n)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(!head || !head->next)  return head;
        ListNode* sub = reverseList(head->next);
        head->next->next = head;
    	head->next = NULL;
    	return sub;
    }
};
```

推荐思路3：三指针遍历， 时间(n) 空间(1)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = nullptr;
        ListNode* cur = head;
        ListNode* next = nullptr;

        while (cur) {
            next = cur->next;
            cur->next = pre;
            // 一个指针要走到下一个不能通过next来走，可能next已经改变，所以需要下一个指针接应
            pre = cur;
            cur = next;
        }
        return pre;//返回新表首结点      
    }
};
```

