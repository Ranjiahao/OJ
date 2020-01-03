<https://leetcode-cn.com/problems/middle-of-the-linked-list/>

给定一个带有头结点 head 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。 

**示例 1：**

```cpp
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```

**示例 2：**

```cpp
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
 提示：给定链表的结点数介于 1 和 100 之间。
```




思路一：将链表输出到数组中，然后取中间元素。 时间O(n)，空间(n)

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        vector<ListNode*> A = {head};
            while (A.back()->next)
            A.push_back(A.back()->next);
        return A[A.size() / 2];
    }
};
```

推荐思路二：快慢指针法。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```

