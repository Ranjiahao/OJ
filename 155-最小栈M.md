<https://leetcode-cn.com/problems/min-stack/>

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

* push(x) -- 将元素 x 推入栈中。
* pop() -- 删除栈顶的元素。
* top() -- 获取栈顶元素。
* getMin() -- 检索栈中的最小元素。



思路1：辅助栈。一个栈存放数据，另一个栈存放前栈中最小的数据， 时间O(1)，空间O(n)
```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
        
    }
    
    void push(int x) {
        if (stk.empty() || minStack.top().min > x) {
            minStack.push(minData(x, 1));
        } else if (minStack.top().min == x) {
            minStack.top().cnt++;
        }
        stk.push(x);
    }
    
    void pop() {
        if (stk.empty()) return;
        int x = stk.top();
        stk.pop();
        if (x == minStack.top().min) {
            if (minStack.top().cnt > 1){
                minStack.top().cnt--;
            } else {
                minStack.pop();
            }
        }
    }
    
    int top() {
        return stk.top();
    }
    
    int getMin() {
        return minStack.top().min;
    }
private:
    struct minData {
        minData(int m, int c) :min(m),cnt(c) {}
        int min;
        int cnt;
    };

    stack<int> stk;
    stack<minData> minStack;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

思路2：只用一个栈。栈中两个数据位合成一个单元，第一个数据位存放当前数据到末尾的最小值，第二个数据位存放当前数据。 时间O(1)，空间O(n)

```cpp
class MinStack {
public:
    void push(int x) {
        if (stk.empty()) {
            stk.push(x);
            stk.push(x);
        } else {
            int temp = stk.top();
            stk.push(x);
            if (x < temp)
                s.push(x);
            else
                s.push(temp);
        }
    }
    
    void pop() {
        s.pop();
        s.pop();
    }
    
    int top() {
        int temp=s.top();
        s.pop();
        int top=s.top();
        s.push(temp);
        return top;
    }

    int getMin() {
        return s.top();
    }
private:
	stack<int> stk;
};
```

思路3：优化思路2，我们发现第二种思路中，会产生一些没有必要的入栈出栈操作(入栈两次、出栈两次)其实可以只用一个变量去保存栈的最小元素。 时间O(1)，空间O(n)

入栈：当需要改变min值的时候，先将原先的min值放入栈中，再将新数据插入栈。此时栈顶等于最值。不需改变min值的时候直接入栈即可。

出栈：当栈顶元素等于最小值时，连续出两次，并更新min值为第二次出栈的元素。若不等于则出一次。

```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
        
    }
    
    void push(int x) {
        if (stk.empty()) {
            stk.push(x);
            min = x;
        } else if (x <= min) {   
            // 将之前的最小值保存
            stk.push(min);
            // 更新最小值
            min = x;
        }
        stk.push(x);
    }
    
    void pop() {
        //如果弹出的值是最小值，那么将下一个元素更新为最小值
        if (stk.top() == min) {
            stk.pop();
            min = stk.top();
        }
        stk.pop();
    }
    
    int top() {
        return stk.top();
    }
    
    int getMin() {
        return min;
    }
private:
    stack<int> stk;
    int min;
};
```

思路4：优化思路3，我们发现思路3入栈更改最小值时仍会多产生一次不必要的入栈，所以下面的方法会彻底去除不必要的入栈。
同样是用一个min变量保存最小值。只不过栈里边我们不去保存原来的值，而是去存储入栈的值和最小值的差值。然后得到之前的最小值的话，我们就可以通过 min 值和栈顶元素得到。 时间O(1)，空间O(n)

入栈：栈为空时，入栈0，并且更改min值为入栈元素(相当于以这个min为参考)；若栈不为空，则入栈x-min(以min为参考的相对值)；入栈后判断x-min，若为负值则说明此时入栈元素是最小元素，则更改min值即可。

出栈：若栈顶元素是负数，则需要更改min值为当前min-栈顶的负数即可。

查看栈顶元素：若栈定为正数则查看时要加上min；若栈顶为负数，则说明最后入栈元素是最小值，此时直接返回min即可。

需要注意的是由于我们使用了减法，这可能导致数据越界(一个整数减一个负数)，所以我们使用了long类型，这在leetcode下能通过，但是由于本地机器可能long和int一样是4个字节，所以可以使用long long

```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {

    }

    void push(int x) {
        if (stk.empty()) {
            min = x;
            stk.push(0);
        } else {
            stk.push(x - min);
            if (x < min)
                min = x;
        }
    }

    void pop() {
        long tmp = stk.top();
        stk.pop();
        if (tmp < 0) {
            min = min - tmp;
        }
    }

    int top() {
        if (stk.top() < 0)
            return min;
        return stk.top() + min;
    }

    int getMin() {
        return min;
    }
private:
    stack<long> stk;
    long min;
};
```
