# Attack on Algorithm - 贪心 🐝 

## 定义

**贪心算法 (Greedy Algorithm) :** 是用计算机来模拟一个贪心的人做出决策的过程 , 这个人十分贪婪 , 每一步行动总是按某种指标选取最优的操作 , 而且他目光短浅 , 总是只看眼前 , 并不考虑以后可能造成的影响

贪心算法不从整体最优上加以考虑 , 而是一步一步进行 , 每一步只以当前情况为基础 , 根据某个优化测度做出局部最优选择 , 从而省去了为找到最优解要穷举所有可能所必须耗费的大量时间

贪心算法可以看做是动态规划的一种极端情况

一般来说 , 这些能够使用贪心算法解决的问题必须满足下面的两个特征 : 

1. **贪⼼选择性质**
2. **最优子结构**

#### 贪心选择性质

**贪心选择性质 :** 指的是一个问题的全局最优解可以通过一系列局部最优解 (贪心选择) 来得到

当进行选择时 , 我们直接做出在当前问题中看来最优的选择 , 而不用去考虑子问题的解 , 在做出选择之后 , 才会去求解剩下的子问题 , 如下图所示

![greedy-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/greedy-01.png?raw=true)

#### 最优子结构

**最优子结构性质 :** 指的是一个问题的最优解包含其子问题的最优解

问题的最优子结构性质是该问题能否用贪心算法求解的关键

举个例子 , 如下图所示 , 原问题 $S=\{a_1,a_2,a_3,a_4\}$ , 在 $a_1$ 步我们通过贪心选择选出一个当前最优解之后 , 问题就转换为求解子问题 $S_{子问题}=\{a_2,a_3,a_4\}$ , 如果原问题 $S$ 的最优解可以由第 $a_1$ 步通过贪心选择的局部最优解和$ S_{子问题}$ 的最优解构成 , 则说明该问题满足最优子结构性质

也就是说 , 如果原问题的最优解包含子问题的最优解 , 则说明该问题满足最优子结构性质

![greedy-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/greedy-02.png?raw=true)

#### 证明

贪心算法有两种证明方法 : 数学归纳法和交换论证法

**数学归纳法 :** 先算得出边界情况 (例如 $n=1$) 的最优解 $F_1$ , 然后再证明 , 对于每个 $n$ , $F_{n+1}$ 都可以由 $F_n$ 推导出结果

**交换论证法 : **从最优解出发 , 在保证全局最优不变的前提下 , 如果交换方案中任意两个元素 (相邻的两个元素后) , 答案不会变得更好 , 则可以推定目前的解是最优解

## 实现

贪心算法的实现步骤 : 

1. **转换问题 :** 将优化问题转换为具有贪心选择性质的问题 , 即先做出选择 , 再解决剩下的一个子问题
2. **贪心选择性质 :** 根据题意选择一种度量标准 , 制定贪心策略 , 选取当前状态下最优选择 , 从而得到局部最优解
3. **最优子结构性质 :** 根据上一步制定的贪心策略 , 将贪心选择的局部最优解和子问题的最优解合并起来 , 得到原问题的最优解

基本代码框架

```python
def greedy_algorithm(problem_input):
    # 1. 初始化解集
    solution = []

    # 2. 按照某种贪心策略排序或预处理输入
    sorted_input = sort_or_process(problem_input)

    # 3. 循环选择局部最优解，并构造最终解
    for item in sorted_input:
        if is_feasible(item, solution):  # 判断是否可加入当前解
            solution.append(item)

    # 4. 返回最终解
    return solution
```

## 例题

- [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)
- [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)
- [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)
- [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)
- [134. 加油站](https://leetcode.cn/problems/gas-station/)
- [135. 分发糖果](https://leetcode.cn/problems/candy/)

