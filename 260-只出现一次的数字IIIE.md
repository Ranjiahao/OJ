https://leetcode-cn.com/problems/single-number-iii/

给定一个整数数组 nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。
示例 :
输入: [1,2,1,3,2,5]
输出: [3,5]

思路：可以使用map最后然后统计次数，或者排序后遍历，这里我们采用最简单的位运算实现，首先将这组整数全部异或，得到数字，找到其中的一个1，再次遍历数组，根据这位数的01分成两组，这样只出现一次的元素就被分开到两个数组中了，然后数组内全不异或取出。  时间O()，空间O()

```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int s = 0;
        for (auto& num : nums) {
            s ^= num;
        }
        int k = s & (-s); // 保留s的最后一个1，并且将其他位变为0
        vector<int> rs = {0, 0};
        for (auto& num : nums) {
            if (num & k) {
                //第一组
                rs[0] ^= num;
            } else {
                //第二组
                rs[1] ^= num;
            }
        }
        return rs;
    }
};
```