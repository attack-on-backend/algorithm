# 20.有效的括号


<extoc></extoc>

## 题目


给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

**示例 1：**

```python
输入：s = "()"
输出：true
```

**示例 2：**

```python
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```python
输入：s = "(]"
输出：false
```

**示例 4：**

```python
输入：s = "([)]"
输出：false
```

**示例 5：**

```python
输入：s = "{[]}"
输出：true
```

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-parentheses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 思路一 - 栈

先将括号入栈 , 遇到匹配的就出栈 , 最后栈为空

注意 : `k` 可能大于数组长度

```python
class Solution:
    def isValid(self, s: str) -> bool:
        _stack = []
        brackets = {'(': ')', '{': '}', '[': ']'}
        for i in s:
            if _stack:
                if brackets.get(_stack[-1], None) == i:
                    _stack.pop()
                    continue
            _stack.append(i)
        return False if _stack else True
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(N)$

### 思路二 - 字符串替换

左括号和又括号一定是对应的 , 所以我们可以直接替换 `()` , `{}` , `[]` 为空字符串 , 如果结果为空字符串就说明有效

注意 : 有可能存在嵌套的括号 , 所以要循环替换

 ```python
class Solution:
    def isValid(self, s: str) -> bool:
        while '()' in s or '{}' in s or '[]' in s:
            s = s.replace('()', '').replace('{}', '').replace('[]', '')
        return False if s else True
 ```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(N)$