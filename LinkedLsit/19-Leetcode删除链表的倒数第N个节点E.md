<https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/submissions/>

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

**示例：**

```cpp
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
说明：给定的 n 保证是有效的。
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```



思路1：遍历一遍求出链表长度，然后根据倒数第n个节点的上一个节点，删除倒数第n个节点。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int count = 0;
        ListNode* cur = head;
        while (cur) {
            count++;
            cur = cur->next;
        }
        // 头删
        if (n == count) {
            ListNode* tmp = head->next;
            delete head;
            return tmp;
        }
        cur = head;
        for(int i = 1; i < count - n; i++) {
            cur = cur->next;
        }
        ListNode* next = cur->next;
        cur->next = next->next;
        delete next;
        return head;
    }
};
```

推荐思路2：双指针遍历一遍找到倒数n+1个节点，删除倒数第n个节点。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        // 由于可能出现第一个节点不好删除，所以引入假节点
        ListNode* fack = new ListNode(-1);
        fack->next = head;
        ListNode* fast = fack;
        ListNode* slow = fack;
        // fast先走n+1步
        while (n + 1) {
            fast = fast->next;
            --n;
        }
        while (fast) {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* next = slow->next;
        slow->next = next->next;
        delete next;
        head = fack->next;
        delete fack;
        return head;
    }
};
```

