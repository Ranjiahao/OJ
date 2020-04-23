https://leetcode-cn.com/problems/lru-cache/

运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。
获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
进阶:
你是否可以在 O(1) 时间复杂度内完成这两种操作？

```cpp
示例:
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );
cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得关键字 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得关键字 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```

思路：哈希+双向链表即可，哈希表中一个key对应一个链表的迭代器

```cpp
class LRUCache {
public:
    LRUCache(int capacity)
        : _capacity(capacity) {}
    
    int get(int key) {
        unordered_map<int, list<pair<int, int>>::iterator>::iterator ht_it = _ht.find(key);
        if (ht_it == _ht.end()) {
            return -1;
        } else {
            list<pair<int, int>>::iterator lt_it = ht_it->second;
            pair<int, int> kv = *lt_it;
            // 删除当前位置后头插到链表中
            _lru_lt.erase(lt_it);
            _lru_lt.push_front(kv);
            _ht[key] = _lru_lt.begin();
            return kv.second;
        }
    }

    void put(int key, int value) {
        unordered_map<int, list<pair<int, int>>::iterator>::iterator ht_it = _ht.find(key);
        if (ht_it == _ht.end()) {
            // 不在则插入
            if (_lru_lt.size() == _capacity) {
                // 缓存满了则删除最少使用的数据
                _ht.erase(_lru_lt.back().first);
                _lru_lt.pop_back();
            }
            // 插入数据
            _lru_lt.push_front(make_pair(key, value));
            _ht[key] = _lru_lt.begin();
        } else {
            // 在则更新，将当前位置值删除，然后头插，并且更新哈希表
            _lru_lt.erase(ht_it->second);
            _lru_lt.push_front(make_pair(key, value));
            _ht[key] = _lru_lt.begin();
        }
    }
private:
    unordered_map<int, list<pair<int, int>>::iterator> _ht;
    int _capacity;
    list<pair<int, int>> _lru_lt;
};
/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```