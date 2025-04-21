# Attack on Algorithm - 栈 🐝 

## 定义

**栈 :** 栈是一种遵循后进先出原则的线性数据结构

**后进先出 (Last In, First Out, LIFO) :** 最后一个进入栈的元素将是第一个被移除的元素

栈是一种受限数据结构 , 只允许新的元素从一个固定的方向插入或者删除 , 这个方位我们叫做**栈顶** , 而从其他位置获取元素是不被允许的

![stack-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/stack-01.png?raw=true)

通常 , 插入操作在栈中被称作**入栈 `push`**  , 总是添加新元素到栈顶 ; 删除操作在栈中被称作**出栈 `pop`**  , 总是从栈顶移除元素

![stack-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/stack-02.gif?raw=true)

栈的基本操作 : 

- `push()` : 将一个元素添加到栈顶
- `pop()` : 移除并返回栈顶元素
- `peek()` : 返回栈顶的元素但是不移除它
- `is_empty()` : 检查栈是否为空
- `size()` : 返回栈中元素的数量

## 实现

栈的实现有两种方式 , 基于两种线性表的实现 : 数组和链表

以下是数组的实现方式

```python
class ArrayStack:
    """基于数组实现的栈"""

    def __init__(self):
        """构造方法"""
        self._stack = []

    def size(self) -> int:
        """获取栈的长度"""
        return len(self._stack)

    def is_empty(self) -> bool:
        """判断栈是否为空"""
        return self.size() == 0

    def push(self, item: int):
        """入栈"""
        self._stack.append(item)

    def pop(self) -> int:
        """出栈"""
        if self.is_empty():
            raise IndexError("栈为空")
        return self._stack.pop()

    def peek(self) -> int:
        """访问栈顶元素"""
        if self.is_empty():
            raise IndexError("栈为空")
        return self._stack[-1]

    def to_list(self) -> list[int]:
        """返回列表用于打印"""
        return self._stack
```

## 例题

- [155. 最小栈](https://leetcode-cn.com/problems/min-stack)
- [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses)
- [150. 逆波兰表达式求值](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation)