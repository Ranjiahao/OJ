<https://leetcode-cn.com/problems/palindrome-linked-list/>

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

**示例 1:**

```cpp
输入: 1->2
输出: false
```

**示例 2:**

```cpp
输入: 1->2->2->1
输出: true
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```



思路1：用栈，有两种可能：

奇数1->2->3，偶数1->2->2->1

先求出整个链表长度并除2找出中点，如果是偶数则中点是后面的节点，将中点之前的数字存入栈中再次判断奇偶，如果是奇数从中点的下一个开始遍历如果相等则出栈，不等则return false。如果是偶数从中点处开始判断。 时间O(n)，空间O(n)

```cpp
class Solution {
public:
    int findlength(ListNode* head) {
        int count = 0;
        while (head) {
            ++count;
            head = head->next;
        }
        return count;
    }

    bool isPalindrome(ListNode* head) {
        if (!head || !head->next) return true;
        stack<int> stk;
        int length = findlength(head);
        ListNode* cur = head;
        for (int i = 0; i < (length / 2); ++i) {
            stk.push(cur->val);
            cur = cur->next;
        }
        if (length % 2) {
            cur = cur->next;
        }
        while (cur) {
            if (stk.top() == cur->val) {
                stk.pop();
                cur = cur->next;
            } else {
                return false;
            }
        }
        return true;
    }
};
```

推荐思路2：快慢指针找到中点，将后半段的链表逆置，这样就可以按照回文的顺序比较了。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(!head || !head->next) return true;
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode* head2 = nullptr;
        ListNode* cur = slow->next;
        slow->next = nullptr;
        while (cur) {
            ListNode* next = cur->next;
            cur->next = head2;
            head2 = cur;
            cur = next;
        }
        while (head2) {
            if (head2->val == head->val) {
                head = head->next;
                head2 = head2->next;
            } else {
                return false;
            }
        }
        return true;
    }
};
```

