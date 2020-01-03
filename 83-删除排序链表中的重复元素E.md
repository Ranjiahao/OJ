<https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/>

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

**示例 1:**

```cpp
输入: 1->1->2
输出: 1->2
```

**示例 2:**

```cpp
输入: 1->1->2->3->3
输出: 1->2->3
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```



推荐思路1：单指针遍历。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return head;
        ListNode* cur = head;
        while (cur->next) {
            if (cur->val == cur->next->val) {
                cur->next=cur->next->next;
            } else {
                cur=cur->next;
            }
        }
        return head;
    }
};
```

思路2：递归。 时间O(n)，空间O(n)

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next)
            return head;
        head->next = deleteDuplicates(head->next);
        if (head->val == head->next->val)
            head = head->next;
        return head;
    }
};
```

