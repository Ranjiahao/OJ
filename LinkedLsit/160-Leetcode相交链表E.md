<https://leetcode-cn.com/problems/intersection-of-two-linked-lists/>

编写一个程序，找到两个单链表相交的起始节点。

```cpp
如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```

思路1：先将headA所有放入哈希表，再遍历headB，若找到已有的节点返回。 时间O(n+m)，空间O(n)

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_set <ListNode*> hash;
        while (headA) {
            hash.insert(headA);
            headA = headA->next;
        }
        while (headB) {
            if (hash.find(headB) != hash.end()){
                return headB;
            }
            headB = headB->next;
        }
        return nullptr;
    }
};
```

思路2：双指针拼接法，同时让ptr1、ptr2走A、B链表，假设ptr1走短的链表先走完，然后让其走长的链表，ptr2走完长的链表后走短的链表，然后两指针相遇就为相交节点。 时间O(n+m)，空间O(1)

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* p = headA,*q = headB;
        while (p!=q) {
            if(p) p=p->next;
            else p=headB;
            if(q) q=q->next;
            else q=headA;
        }
        return p;
    }
};
```