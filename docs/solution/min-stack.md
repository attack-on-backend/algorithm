# 155.最小栈


<extoc></extoc>

## 题目

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

`push(x)` —— 将元素 x 推入栈中。
`pop()` —— 删除栈顶的元素。
`top()` —— 获取栈顶元素。
`getMin()` —— 检索栈中的最小元素。


示例:
```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

提示：

- `pop`、`top` 和 `getMin` 操作总是在 非空栈 上调用。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/min-stack
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 思路一 - 暴力解

使用 `list` 来模拟栈操作

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self._stack = []

    def push(self, x: int) -> None:
        self._stack.append(x)

    def pop(self) -> None:
        self._stack.pop()

    def top(self) -> int:
        return self._stack[-1]

    def getMin(self) -> int:
        min_num = self._stack[0]
        for i in range(1, len(self._stack)):
            if self._stack[i] < min_num:
                min_num = self._stack[i]
        return min_num
```

**时间复杂度**：

- `push(x)` : $O(1)$
- `pop()` : $O(1)$
- `top()` : $O(1)$
- `getMin()` : $O(N)$ 

### 思路二 - 优化 getMin()

基于思路一 , 将最小元素提前存储 , 在栈进行删除和新增的时候维护最小元素

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self._stack = []
        self.min = None 

    def push(self, x: int) -> None:
        self._stack.append(x)
        if self.min is None or self.min > x:
            self.min = x

    def pop(self) -> None:
        x = self._stack.pop()
        if x == self.min:
            self.min = min(self._stack) if self._stack else None

    def top(self) -> int:
        return self._stack[-1]

    def getMin(self) -> int:
        return self.min
```

**时间复杂度**：

- `push(x)` : $O(1)$  
- `pop()` : $O(N)$ 
- `top()` : $O(1)$
- `getMin()` : $O(1)$  

