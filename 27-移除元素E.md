<https://leetcode-cn.com/problems/remove-element/>

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**示例 1:**

```cpp
给定 nums = [3,2,2,3], val = 3,
函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
你不需要考虑数组中超出新长度后面的元素。
```

**示例 2:**

```cpp
给定 nums = [0,1,2,2,3,0,4,2], val = 2,
函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
注意这五个元素可为任意顺序。
你不需要考虑数组中超出新长度后面的元素。
```



思路一：快慢指针。 时间O(n)，空间O(1)

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;
        int n = nums.size();
        while (i < n) {
            if (nums[i] == val) {
                nums[i] = nums[n - 1]; 
                n--;
            } else {
                i++;
            }
        }
        return n;
    }
};
```

思路二：当我们遇到 nums[i] = val时，我们可以将当前元素与最后一个元素进行交换，并释放最后一个元素。这实际上使数组的大小减少了 1。 时间O(n)，空间O(1)
如果等于val的值比较少，思路二会转化效率些。例如1 2 3 4，val =2。解法一而循环中将调用3次赋值。而解法二中，仅当等于val的时候赋值1次

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int fast = 0;
        int slow = 0;
        while (fast < nums.size()) {
            if (nums[fast] != val) {
                nums[slow++] = nums[fast];
            }
            fast++;
        }
        return slow;
    }
};
```