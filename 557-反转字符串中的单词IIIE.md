https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

示例 1:
输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc" 
注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格

思路1：常规思路，注意最后一次逆置过程。  时间O(n) 空间O(n)
```cpp
class Solution {
public:
    string reverseWords(string s) {
        int l = s.find_first_not_of(' ');
        int r = s.find_first_of(' ', l);
        while ( l != string::npos) {
            r == string::npos ? reverse(s.begin() + l, s.end()) : reverse(s.begin() + l, s.begin() + r);
            l = s.find_first_not_of(' ', r);
            r = s.find_first_of(' ', l);
        }
        return s;
    }
};
```

思路2：stringstream求解。  时间O(n) 空间O(n)

```cpp
class Solution {
public:
    string reverseWords(string s) {
        istringstream ss(s);
        string res, str;
        while (ss >> str) {
            reverse(str.begin(), str.end());
            res += str + " ";
        }
        return res.substr(0, res.size() - 1);
    }
};
```

