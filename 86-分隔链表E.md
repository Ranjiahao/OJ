<https://leetcode-cn.com/problems/partition-list/solution/>

给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

**示例:**

```cpp
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```

思路：创建两个假头节点，分别代表大于等于x组，和小于等于x组。遍历原链表，最后将大于组连在小于组后面。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode less_head(-1);
        ListNode more_head(-1);
        ListNode* less_ptr = &less_head;
        ListNode* more_ptr = &more_head;
        while (head) {
            if(head->val < x) {
                less_ptr->next = head;
                less_ptr = head;
            } else {
                more_ptr->next=head;
                more_ptr=head;
            }
            head=head->next;
        }
        more_ptr->next=nullptr;
        less_ptr->next=more_head.next;
        return less_head.next;
    }
};
```

