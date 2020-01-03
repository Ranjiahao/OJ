<<https://leetcode-cn.com/problems/merge-two-sorted-lists/description/>>

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例：**

```cpp
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```



推荐思路1：创建新链表并记录链表尾，循环取l1、l2头节点较小的节点放入新链表中。 时间O(n+m)，空间O(1)

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL)
            return l2;
        if (l2 == NULL)
            return l1;
        ListNode* last = nullptr;
        ListNode* result = nullptr;
        while (l1 && l2) {
            if(l1->val > l2->val) {
                if (last == nullptr) {
                    last = l2;
                    result = l2;
                } else {
                    last->next = l2;
                    last = last->next;
                }
                l2 = l2->next;
            } else {
                if (last == nullptr) {
                    last = l1;
                    result = l1;
                } else {
                    last->next = l1;
                    last = last->next;
                }
                l1 = l1->next;
            }
        }
        if (l1 == nullptr) {
            last->next = l2;
        }
        else if (l2 == nullptr) {
            last->next = l1;
        }
        return result;
    }
};
```

思路2：递归。 时间O(n+m)，空间O(n+m)

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL) return l2;
        if (l2 == NULL) return l1;
        if (l1->val < l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        } else {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```
