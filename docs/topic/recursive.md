# Attack on Algorithm - 递归 🐝 

## 定义

**递归 (Recursion) :** 指的是一种通过重复将原问题分解为同类的子问题而解决的方法 , 在绝大数编程语言中 , 可以通过在函数中再次调用函数自身的方式来实现递归

以阶乘计算为例

```python
def fact(n):
    if n == 0:
        return 1
    return n * fact(n - 1)
  
# 以n = 6为例, 调用过程如下
fact(6)
= 6 * fact(5)
= 6 * (5 * fact(4))
= 6 * (5 * (4 * fact(3)))
= 6 * (5 * (4 * (3 * fact(2))))
= 6 * (5 * (4 * (3 * (2 * fact(1)))))
= 6 * (5 * (4 * (3 * (2 * (1 * fact(0))))))
= 6 * (5 * (4 * (3 * (2 * (1 * 1)))))
= 6 * (5 * (4 * (3 * (2 * 1))))
= 6 * (5 * (4 * (3 * 2)))
= 6 * (5 * (4 * 6))
= 6 * (5 * 24)
= 6 * 120
= 720
```

递归的计算过程分为两个部分 , 先逐层向下调用自身 , 即递推过程 , 指的是将原问题一层一层地分解为与原问题形式相同、规模更小的子问题 , 直到达到结束条件时停止 , 此时返回最底层子问题的解

![recursive-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/recursive-01.png?raw=true)

然后再向上逐层返回结果 , 直到返回原问题的解 , 即回归过程 , 指的是从最底层子问题的解开始 , 逆向逐一回归 , 最终达到递推开始时的原问题 , 返回原问题的解

![recursive-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/recursive-02.png?raw=true)

因为解决原问题和不同规模的小问题往往使用的是相同的方法 , 所以就产生了函数调用函数自身的情况 , 这也是递归的定义所在

## 实现

递归算法的实现步骤 : 

1. **写出递推公式 :** 找到将原问题分解为子问题的规律 , 并且根据规律写出递推公式
2. **明确终止条件 :** 推敲出递归的终止条件 , 以及递归终止时的处理方法
3. **将递推公式和终止条件翻译成代码 :**
   1. 定义递归函数 (明确函数意义、传入参数、返回结果等)
   2. 书写递归主体 (提取重复的逻辑 , 缩小问题规模)
   3. 明确递归终止条件 (给出递归终止条件 , 以及递归终止时的处理方法)

#### 普通递归

普通递归指递归调用不是函数的最后一步操作 , 在递归返回时需要进行额外计算 , 因此每层调用都需要保存在调用栈中

```python
def recursive(n):
    """递归"""
    # 终止条件
    if n == 1:
        return 1
    # 递: 递归调用
    res = recursive(n - 1)
    # 归: 返回结果
    return n + res
```

#### 尾递归

尾递归指递归调用是函数的最后一步操作 , 不需要在返回后做任何处理 , 因此在有的语言中会有尾递归优化 , 不增加调用栈的深度

```python
def tail_recursive(n, res):
    """尾递归"""
    # 终止条件
    if n == 0:
        return res
    # 尾递归调用
    return tail_recursive(n - 1, res + n)
```

#### 调用栈

递归函数每次调用自身时 , 系统都会为新开启的函数分配内存 , 以存储局部变量、调用地址和其他信息等 , 这将导致两方面的结果

- 函数的上下文数据都存储在称为 "栈帧空间" 的内存区域中 , 直至函数返回后才会被释放 , 因此 , **递归通常比迭代更加耗费内存空间**
- 递归调用函数会产生额外的开销 , **因此递归通常比循环的时间效率更低**

所以在使用递归时我们要避免栈溢出

## 应用

#### 分治算法

递归时一种解决问题的思维方式 , 分治算法很大程度上是基于递归的

如快速排序、归并排序等 , 通过递归将问题划分为子问题

#### DFS

用于图和树的遍历 , 例如二叉树的前序、中序、后序遍历

#### 斐波那契数列

经典递归问题 , 但需注意重复计算问题 , 可使用记忆化优化

## 例题

- [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)
- [LCR 187. 破冰游戏](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)
- [50. Pow(x, n)](https://leetcode.cn/problems/powx-n/)
