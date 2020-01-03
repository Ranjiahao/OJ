<https://leetcode-cn.com/problems/copy-list-with-random-pointer/>

给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的深拷贝。

**提示：**必须返回给定头的拷贝作为对克隆列表的引用

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node() {}

    Node(int _val, Node* _next, Node* _random) {
        val = _val;
        next = _next;
        random = _random;
    }
};
*/
```



思路1：借助哈希保存节点信息。 时间O(n)，空间O(n)

1. 遍历随机链表，建立每一个节点对应各自拷贝出来节点的映射，保存在unordered_map中
2. 再次遍历原链表，复制next和random指针

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) return head;
        Node* cur = head;
        unordered_map<Node*, Node*> map;
        while (cur) {
            Node* copy = new Node(cur->val);
            map[cur] = copy;
            cur = cur->next;
        }
        cur = head;
        while (cur != nullptr) {
            map[cur]->next = map[cur->next];
            map[cur]->random = map[cur->random];
            cur = cur->next;
        }
        return map[head];
    }
};
```

思路2：原地复制。 时间O(n)，空间O(1)

复制节点，同时将复制节点链接到原节点后面；连接random；分离链表

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) return head;
        Node* cur = head;
        //1. 复制节点，同时将复制节点链接到原节点后面
        while (cur) {
            Node* copy = new Node(cur->val, nullptr, nullptr);
            copy->next = cur->next;
            cur->next = copy;
            cur = copy->next;
        }
        //2. 连接random
        cur = head;
        while (cur) {
            if (cur->random) {
                cur->next->random = cur->random->next;
            }            
            cur = cur->next->next;
        }
        //3. 分离链表
        cur = head;
        Node* newHead = head->next;
        Node* newcur = newHead;
        while (cur) {
            cur->next = cur->next->next;     
            if (newcur->next)
                newcur->next = newcur->next->next;
            cur = cur->next;
            newcur = newcur->next;
        }
        return newHead;
    }
};
```



















