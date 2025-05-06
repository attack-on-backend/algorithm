# Attack on Algorithm - 分治 🐝 

## 定义

**分治算法 (Divide and Conquer) :** 字面上的解释是分而治之 , 就是把一个复杂的问题分成两个或更多的相同或相似的子问题 , 直到最后子问题可以简单的直接求解 , 原问题的解即子问题的解的合并

简单来说 , 分治算法的基本思想就是 : **把规模大的问题不断分解为子问题 , 使得问题规模减小到可以直接求解为止**

![divide-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/divide-01.png?raw=true)

分治算法能够解决的问题 , 一般需要满足以下 4 个条件 :

1. **可分解 :** 原问题可以分解为若干个规模较小的相同子问题
2. **子问题可独立求解 :** 分解出来的子问题可以独立求解 , 即子问题之间不包含公共的子子问题
3. **具有分解的终止条件 :** 当问题的规模足够小时 , 能够用较简单的方法解决
4. **可合并 :** 子问题的解可以合并为原问题的解 , 并且合并操作的复杂度不能太高 , 否则就无法起到减少算法总体复杂度的效果了

## 方法

使用分治算法解决问题主要分为 3 个步骤 : 

1. **分解 :** 把要解决的问题分解为成若干个规模较小、相对独立、与原问题形式相同的子问题
2. **求解 :** 递归求解各个子问题
3. **合并 :** 按照原问题的要求 , 将子问题的解逐层合并构成原问题的解 

其中第 1 步中将问题分解为若干个子问题时 , 最好使子问题的规模大致相同 , 换句话说 , 将一个问题分成大小相等的 $k$ 个子问题的处理方法是行之有效的 , 在许多问题中 , 可以取 $k=2$ 这种使子问题规模大致相等的做法是出自一种平衡子问题的思想 , 它几乎总是比子问题规模不等的做法要好

其中第 2 步的递归求解各个子问题指的是按照同样的分治策略进行求解 , 即通过将这些子问题分解为更小的子子问题来进行求解 , 就这样一直分解下去 , 直到分解出来的子问题简单到只用常数操作时间即可解决为止

在完成第 2 步之后 , 最小子问题的解可用常数时间求得 , 然后我们再按照递归算法中回归过程的顺序 , 由底至上地将子问题的解合并起来 , 逐级上推就构成了原问题的解

基本代码框架

```python
def divide_conquer(problem, *args):
    # 1. 终止条件：当问题规模足够小或可以直接解决时
    if problem is simple_enough:
        return solve_directly(problem)

    # 2. 分解（Divide）：将原问题分解为若干子问题
    sub_problems = split_into_sub_problems(problem)

    # 3. 求解（Conquer）：递归地解决每个子问题
    sub_solutions = [divide_conquer(sub_problem, *args) for sub_problem in sub_problems]

    # 4. 合并（Combine）：将子问题的解合并为原问题的解
    final_solution = combine_solutions(sub_solutions)

    return final_solution
```

## 例题

- [1763. 最长的美好子字符串](https://leetcode.cn/problems/longest-nice-substring/)
- [LCR 158. 库存管理 II](https://leetcode.cn/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)
- [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)



