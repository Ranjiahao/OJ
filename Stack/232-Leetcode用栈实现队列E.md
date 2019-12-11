<https://leetcode-cn.com/problems/implement-queue-using-stacks/>

使用栈实现队列的下列操作：
* push(x) -- 将一个元素放入队列的尾部。
* pop() -- 从队列首部移除元素。
* peek() -- 返回队列首部的元素。
* empty() -- 返回队列是否为空。

说明:

* 你只能使用标准的栈操作 -- 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。

* 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
* 假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。



思路：使用两个栈。时间入队O(1)，出队均摊O(1)最坏O(n)，返回队首元素O(1)，判空O(1)

```cpp
class MyQueue {
public:
    /** Initialize your data structure here. */
    MyQueue() {

    }

    /** Push element x to the back of queue. */
    void push(int x) {
        if (inPut.empty())
            front = x;
        inPut.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if (outPut.empty()) {
            while (!inPut.empty()) {
                outPut.push(inPut.top());
                inPut.pop();
            }   
        }
        int tmp = outPut.top();
        outPut.pop();
        return tmp; 
    }

    /** Get the front element. */
    int peek() {
        if (outPut.empty())
			return front;
        return outPut.top();
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return outPut.empty() && inPut.empty();
    }
private:
    stack<int> outPut;
    stack<int> inPut;
    int front;
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

