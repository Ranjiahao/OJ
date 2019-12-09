<https://leetcode-cn.com/problems/linked-list-cycle/>

给定一个链表，判断链表中是否有环。一个节点为没有环。

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

思路1：哈希表，将链表循环插入哈希表中直到哈希表size不变则有环，若遍历完链表则为无环。 时间O(n)，空间O(n)

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        set<ListNode*> hash;
        ListNode* q = head;
        while (q) {
            int size = hash.size();
            hash.insert(q);
            if (size == hash.size()) {
                return true;
            }
            q = q->next;
        }
        return false;
    }
};
```

思路2：快慢指针，一个指针走一步，一个指针走两步，若存在环 fast 和 slow 会相遇。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    bool hasCycle(ListNode* head) {
        if (head == nullptr)
            return false;

        ListNode* fast = head;
        ListNode* slow = head;

        do {
            fast = fast->next;
            if (fast == nullptr)
                return false;
            fast = fast->next;
            if (fast == nullptr)
                return false;
            slow = slow->next;
        } while (fast != slow);
        return true;
    }
};
```