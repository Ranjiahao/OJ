<https://leetcode-cn.com/problems/design-circular-queue/>

设计你的循环队列实现。 循环队列是一种线性数据结构，其操作表现基于 FIFO（先进先出）原则并且队尾被连接在队首之后以形成一个循环。它也被称为“环形缓冲器”。
循环队列的一个好处是我们可以利用这个队列之前用过的空间。在一个普通队列里，一旦一个队列满了，我们就不能插入下一个元素，即使在队列前面仍有空间。但是使用循环队列，我们能使用这些空间去存储新的值。
你的实现应该支持如下操作：

* MyCircularQueue(k): 构造器，设置队列长度为 k 。
* Front: 从队首获取元素。如果队列为空，返回 -1 。
* Rear: 获取队尾元素。如果队列为空，返回 -1 。
* enQueue(value): 向循环队列插入一个元素。如果成功插入则返回真。
* deQueue(): 从循环队列中删除一个元素。如果成功删除则返回真。
* isEmpty(): 检查循环队列是否为空。
* isFull(): 检查循环队列是否已满。

思路：采用vector实现，一个变量表示第一个元素的下标，一个变量表示最后一个元素的下一个位置。在循环队列中会有一个问题：无法区分队空和队满的状态，因为队空和队满的条件都是rear==front，解决方法有少用一个存储单元，设置一个标志变量，设置一个变量size三种方法，这里我们用变量size来控制队列中的元素个数，就可以区分队列空和满。 时间O(1)，空间O(n)
```cpp
class MyCircularQueue {
public:
    /** Initialize your data structure here. Set the size of the queue to be k. */
    MyCircularQueue(int k) {
        data.resize(k);
        head = 0;
        tail = 0;  
        size = 0;
    }

    /** Insert an element into the circular queue. Return true if the operation is successful. */
    bool enQueue(int value) {
        if (size < data.size()) {
            data[tail] = value;
            tail = (tail + 1) % data.size();
            ++size;
            return true;
        }
        return false;
    }

    /** Delete an element from the circular queue. Return true if the operation is successful. */
    bool deQueue() {
        if (size > 0) {
            head = (head + 1) % data.size();
            --size;
            return true;
        }
        return false;
    }

    /** Get the front item from the queue. */
    int Front() {
        if (size == 0)  
            return -1;
        return data[head];
    }

    /** Get the last item from the queue. */
    int Rear() {
        if (size == 0)
            return -1;
        return data[(tail - 1 + data.size()) % data.size()];
    }

    /** Checks whether the circular queue is empty or not. */
    bool isEmpty() {
        return size == 0;
    }

    /** Checks whether the circular queue is full or not. */
    bool isFull() {
        return size == data.size();
    }
private:
    vector<int> data;
    int head;
    int tail;    
    int size;
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```
