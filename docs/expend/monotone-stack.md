# Attack on Algorithm - 单调栈 🐝 

## 定义

**单调栈 (Monotone Stack) :** 一种特殊的栈 , 在栈的先进后出规则基础上 , 要求从 **栈顶** 到 **栈底** 的元素是单调递增 (或者单调递减) , 其中满足从栈顶到栈底的元素是单调递增的栈 , 叫做单调递增栈 , 满足从栈顶到栈底的元素是单调递减的栈 , 叫做单调递减栈

单调栈可以在时间复杂度为 $O(n)$ 的情况下 , 求解出某个元素左边或者右边第一个比它大或者小的元素

所以单调栈一般用于解决一下几种问题 : 

- 寻找左侧第一个比当前元素大的元素
- 寻找左侧第一个比当前元素小的元素。
- 寻找右侧第一个比当前元素大的元素
- 寻找右侧第一个比当前元素小的元素

## 实现

### 单调栈递增

**单调递增栈 :** 只有比栈顶元素小的元素才能直接进栈 , 否则需要先将栈中比当前元素小的元素出栈 , 再将当前元素入栈。

这样就保证了 , 栈中保留的都是比当前入栈元素大的值 , 并且从栈顶到栈底的元素值是单调递增的

单调递增栈的入栈、出栈过程如下：

- 假设当前进栈元素为 $x$ , 如果 $x$x 比栈顶元素小 , 则直接入栈
- 否则从栈顶开始遍历栈中元素 , 把小于 $x$ 或者等于 $x$ 的元素弹出栈 , 直到遇到一个大于 $x$ 的元素为止 , 然后再把 $x$ 压入栈中

![monotone-stack-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/monotone-stack-01.png?raw=true)

代码模板

```python
def monotone_incr_stack(nums):
    stack = []
    for num in nums:
        while stack and num >= stack[-1]:
            stack.pop()
        stack.append(num)
```

### 单调栈递减

**单调递减栈 :** 只有比栈顶元素大的元素才能直接进栈 , 否则需要先将栈中比当前元素大的元素出栈 , 再将当前元素入栈

这样就保证了 , 栈中保留的都是比当前入栈元素小的值 , 并且从栈顶到栈底的元素值是单调递减的

单调递减栈的入栈、出栈过程如下 : 

- 假设当前进栈元素为 $x$ , 如果 $x$ 比栈顶元素大 , 则直接入栈
- 否则从栈顶开始遍历栈中元素 , 把大于 $x$ 或者等于 $x$ 的元素弹出栈 , 直到遇到一个小于 $x$ 的元素为止 , 然后再把 $x$ 压入栈中

![monotone-stack-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/monotone-stack-02.png?raw=true)

代码模板

```python
def monotone_decr_stack(nums):
    stack = []
    for num in nums:
        while stack and num <= stack[-1]:
            stack.pop()
        stack.append(num)
```

## 例题

- [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)
- [503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)
- [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)



