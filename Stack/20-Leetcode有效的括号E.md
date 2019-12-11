<https://leetcode-cn.com/problems/valid-parentheses/>

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。



思路：哈希表建立右括号到左括号的映射，遍历string，遇到左括号入栈遇到右括号判断是否与栈顶对应，遍历完后若栈为空则是有效括号。 时间O(n)，空间O(n)

```cpp
class Solution {
public:
    bool isValid(string s) {
        if(s.size() % 2) return false;
        map<char,char> wordbook;
        wordbook[')'] = '(';
        wordbook[']'] = '[';
        wordbook['}'] = '{';
        stack<char> stk;
        for(auto& c : s) {
            if(c == '[' || c == '{' || c == '(') {
                stk.push(c);
            } else if (c == ']' || c == '}' || c == ')') {
                if(stk.empty()) return false;
                if(wordbook[c] == stk.top()) {
                    stk.pop();
                } else {
                    return false;
                }
            }
        }
        return stk.empty();
    }
};
```