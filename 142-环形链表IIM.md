<https://leetcode-cn.com/problems/linked-list-cycle-ii/>

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

说明：不允许修改给定的链表。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```



思路1：哈希表，用 Set 保存已经访问过的节点，遍历整个列表并返回第一个出现重复的节点。 时间O(n)，空间O(n)

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*> myset;
        while (head != nullptr) {
            if(myset.count(head) > 0)
                return head;
            else
                myset.insert(head);
            head = head->next;
        }
        return nullptr;
    }
};
```

思路2：双指针遍历法。 时间O(n)，空间O(1)

将节点头标记为-n，下一个为-n+1...直到入环第一个节点标记为0，下一个节点标记为1...标记完整个环，最后一个节点标记c-1，因此c节点和0节点重合。

起始位置分析：让慢指针先走到0节点，此时快指针已经走到了n%c节点。

重合位置分析：快指针追满指针，追击距离是c-n%c，速度差为1，所以当慢指针走到c-n%c时，快指针走到2*(c-n%c)+n%c-c。两个刚好重合。

相交节点分析：由于起始位置分析中慢指针走n步到0，快指针走2n步到n%c可知n=n%c。重合位置是c-n%c距离相交节点n%c，所以让一个指针从-n走到0，同时让另一个指针从相遇位置开始走，相遇节点即为所求。

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        do {
            if (fast && fast->next) {
                fast = fast->next->next;
                slow = slow->next;
            } else {
                return nullptr;
            }
        } while (fast != slow);
        slow = head;
        while (fast != slow) {
            slow = slow->next;
            fast = fast->next;
        }
        return fast;
    }
};
```



