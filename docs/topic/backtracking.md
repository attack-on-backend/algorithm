# Attack on Algorithm - 回溯 🐝 

## 定义

**回溯算法 (Backtracking) :** 一种能避免不必要搜索的穷举式的搜索算法 , 采用试错的思想 , 在搜索尝试过程中寻找问题的解 , 当探索到某一步时 , 发现原先的选择并不满足求解条件 , 或者还需要满足更多求解条件时 , 就退回一步(回溯) 重新选择 , 这种走不通就退回再走的技术称为回溯法 , 而满足回溯条件的某个状态的点称为回溯点

简单来说 , 回溯算法采用了一种 **走不通就回退** 的算法思想

回溯算法通常用简单的递归方法来实现 , 在进行回溯过程中更可能会出现两种情况 : 

1. 找到一个可能存在的正确答案
2. 在尝试了所有可能的分布方法之后宣布该问题没有答案

回溯算法的决策过程如下图 : 

![backtracking-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/backtracking-01.png?raw=true)


## 实现

回溯算法的基本思想是 : **以深度优先搜索的方式 , 根据产生子节点的条件约束 , 搜索问题的解 , 当发现当前节点已不满足求解条件时 , 就回溯返回 , 尝试其他的路径**

回溯算法的实现步骤如下：

1. **明确所有选择 :** 画出搜索过程的决策树 , 根据决策树来确定搜索路径
2. **明确终止条件 :** 推敲出递归的终止条件 , 以及递归终止时的要执行的处理方法
3. **将决策树和终止条件翻译成代码 :**
   1. 定义回溯函数 (明确函数意义、传入参数、返回结果等)
   2. 书写回溯函数主体 (给出约束条件、选择元素、递归搜索、撤销选择部分)
   3. 明确递归终止条件 (给出递归终止条件，以及递归终止时的处理方法

基本代码框架

```python
def backtrack(start=0, path=None, used=None):
    # 初始化参数(通常在首次调用时设置)
    if path is None:
        path = []
    if used is None:
        used = [False] * len(nums)

    # 结束条件: 找到了一个完整的解
    if meet_end_condition(path):
        add_to_result(path)
        return

    # 遍历所有可能的选择
    for i in range(start, len_choice_space()):
        # 剪枝: 如果当前选择不合法,跳过
        if should_prune(i, path):
            continue
        # 做出选择
        make_choice(i, path, used)
        # 递归进入下一层决策
        backtrack(next_start(i), path, used)
        # 撤销选择(回溯的关键)
        undo_choice(i, path, used)
```

## 例题

- [46. 全排列](https://leetcode.cn/problems/permutations/)
- [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/)
- [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)



