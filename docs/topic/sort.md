# Attack on Algorithm - 排序算法 🐝 

## 前言

开始之前先写一个时间测试装饰器


```python
import time
def cal_time(func):
    def wrapper(*args, **kwargs):
        t1 = time.time()
        result = func(*args, **kwargs)
        t2 = time.time()
        print("%s running time: %s secs." % (func.__name__, t2-t1))
        return result
    return wrapper
```

## 冒泡排序

工作流程 : 

1. 比较相邻的元素 , 如果第一个比第二个大 , 就交换它们
2. 对每一对相邻元素作同样的工作 , 从列表的开始到结尾依次进行 , 如果列表长度为n , 那么总共走(n-1)躺

图解 : 

![bubble_sort](https://github.com/attack-on-backend/algorithm/blob/master/assert/bubble_sort.gif?raw=true)


代码实现 : 

```python
import random
from timewrap import *
# 第一版
@cal_time
def bubble_sort_1(li):
    # 总共走n-1躺
    for n in range(len(li) - 1):
		# 每一躺比较(总长度-n-1次)
        for j in range(0, len(li) - n - 1):
            if li[j] > li[j+1]:
                li[j], li[j+1] = li[j+1], li[j]

# 在第一版的基础进行优化                
@cal_time
def bubble_sort_2(li):
    # 总共走(n-1)躺
    for n in range(len(li) - 1):
        # 没有改变说明元素位置正确,为了防止更多不必要的比较
        change = False
        # 每一躺比较(总长度-n-1次)
        for j in range(0, len(li) - n - 1):
            if li[j] > li[j+1]:
                li[j], li[j+1] = li[j+1], li[j]
                change = True
        if not change:
            return

li = list(range(10000))
random.shuffle(li)
bubble_sort_1(li)
bubble_sort_2(li)
'''
执行结果:
bubble_sort_1 running time: 29.570897817611694 secs.
bubble_sort_2 running time: 0.002001523971557617 secs.
'''
```

## 选择排序

工作流程 : 

每次从待排序的数据元素中选出最小 (或最大) 的一个元素 , 存放在序列的起始位置 , 直到全部待排序的数据元素排完

代码实现 : 

```python
import random
from timewrap import *
@cal_time
def select_sort(li):
    # 总共走(n-1)躺
    for n in range(len(li) - 1):
        # 最小数的位置
        min_loc = n
        for j in range(n + 1, len(li) - 1):
            if li[j] < li[min_loc]:
                min_loc = j
        li[n], li[min_loc] = li[min_loc], li[n]

li = list(range(10000))
random.shuffle(li)
select_sort(li)
'''
执行结果:
select_sort running time: 18.08176565170288 secs.
'''
```

## 插入排序

工作流程 : 

1. 存在一个已经有序的数据序列
2. 将后续数据按照要求依次一个个插入有序序列中

图解 : 

![insert_sort](https://github.com/attack-on-backend/algorithm/blob/master/assert/insert_sort.gif?raw=true?raw=true)

代码实现 : 

```python
import random
from timewrap import *
@cal_time
def insert_sort(li):
    # 循环无序区进行排序
    for n in range(1, len(li)):
        tmp = li[n]
        # 指向有序区最后位置
        j = n - 1
        while li[j] > tmp and j >= 0:
            li[j+1] = li[j]
            j -= 1
        li[j+1] = tmp

li = list(range(10000))
random.shuffle(li)
insert_sort(li)
'''
执行结果:
insert_sort running time: 18.230905055999756 secs.
'''
```

## 快速排序

工作流程 :

1. 取一个元素P(第一个元素) , 使元素P归位
2. 列表被P分成两部分 , 左边都小于P , 右边都大于P
3. 递归完成排序

图解 : 

![quick_sort](https://github.com/attack-on-backend/algorithm/blob/master/assert/quick_sort.gif?raw=true)

代码实现 : 

```python
import random
from timewrap import *
import copy
import sys
# 修改递归最大层数
sys.setrecursionlimit(100000)

def partition(li, left, right):
    """
    进行分区
    """
    # 防止出现最坏情况
    # ri = random.randint(left, right)
    # li[left], li[ri] = li[ri], li[left]
    tmp = li[left]
    while left < right:
        while left < right and li[right] >= tmp:
            right -= 1
        li[left] = li[right]
        while left < right and li[left] <= tmp:
            left += 1
        li[right] = li[left]
    li[left] = tmp
    return left

def _quick_sort(li, left, right):
    """
    进行排序
    """
    if left < right:    # 至少有两个元素
        mid = partition(li, left, right)
        _quick_sort(li, left, mid-1)
        _quick_sort(li, mid+1, right)

@cal_time
def quick_sort(li):
    return _quick_sort(li, 0, len(li)-1)

li = list(range(10000))
random.shuffle(li)
quick_sort(li)
'''
执行结果:
quick_sort running time: 0.10507440567016602 secs.
'''
```

## 堆排序

**堆分类**

1. 大根堆 : 一颗完全二叉树 , 满足任一节点都比其孩子节点大
2. 小根堆 : 一颗完全二叉树 , 满足任一节点都比其孩子节点小

工作流程 : 

1. 建立堆 , 已完成调整 (以构建大根堆为例)
2. 得到堆顶元素 , 为最大元素
3. 去掉堆顶 , 将堆最后一个元素放到堆顶 , 随后重新调整 (挨个出数)
4. 一直重复3 , 直到堆为空

挨个出数规则 : 找最后一个数作为棋子 , 然后取堆顶的值 , 存放最后 , 依次执行取出

图解 : 

1.构建大根堆 , 索引按照从上倒下 , 从左到右依次递增

![构造大根堆](https://github.com/attack-on-backend/algorithm/blob/master/assert/构造大根堆.gif?raw=true)

2.挨个出数

![挨个出数](https://github.com/attack-on-backend/algorithm/blob/master/assert/挨个出数.gif?raw=true)

代码实现 : 

```python
from timewrap import *
import random

def sift(li, low, high):
    """
    :param li:
    :param low: 堆根节点的位置
    :param high: 堆最有一个节点的位置
    :return:
    """
    # 父亲的位置
    i = low
    # 孩子的位置
    j = 2 * i + 1
    # 原父亲
    tmp = li[low]
    while j <= high:
        # 如果右孩子存在并且右孩子更大
        if j + 1 <= high and li[j+1] > li[j]:
            j += 1
        # 如果原父亲比孩子小
        if tmp < li[j]:
            # 把孩子向上移动一层
            li[i] = li[j]
            i = j
            j = 2 * i + 1
        else:
            break
    li[i] = tmp

@cal_time
def heap_sort(li):
    n = len(li)
    # 建堆
    for i in range(n//2-1, -1, -1):
        sift(li, i, n-1)
    # 挨个出数
    for j in range(n-1, -1, -1):    # j表示堆最后一个元素的位置
        li[0], li[j] = li[j], li[0]
        # 堆的大小少了一个元素(j-1)
        sift(li, 0, j-1)

li = list(range(10000))
random.shuffle(li)
heap_sort(li)
'''
执行结果:
heap_sort running time: 0.3037147521972656 secs.
'''
```

Python中有一个内置模块`heapq`可以帮助我们快速实现对排序

```python
import heapq
import random
from timewrap import *

@cal_time
def heap_sort(li):
    heapq.heapify(li)
    n = len(li)
    new_li = []
    for i in range(n):
        new_li.append(heapq.heappop(li))
    return new_li

li = list(range(10000))
random.shuffle(li)
# 小根堆
heap_sort(li)
# 大根堆,直接利用方法nlargest
heapq.nlargest(100, li)
```

## 归并排序

假设现在的列表分成两段有序 , 如何将其合成为一个有序的列表

工作流程 : 

1. 分解列表 , 直至分解为一个个只有一个元素的列表
2. 比较两段序列中索引相同的值的大小 , 符合条件就进行交换 , 如小的在左 , 大的在右
3. 进行合并 , 重复2操作 , 直至合并为一个列表得出结果

图解 : 

![merge_sort](https://github.com/attack-on-backend/algorithm/blob/master/assert/merge_sort.png?raw=true)

代码实现 : 

```python
import random
def merge(li, low, mid, high):
    i = low
    j = mid + 1
    ltmp = []
    while i <= mid and j <= high:
        if li[i] < li[j]:
            ltmp.append(li[i])
            i += 1
        else:
            ltmp.append(li[j])
            j += 1
    while i <= mid:
        ltmp.append(li[i])
        i += 1
    while j <= high:
        ltmp.append(li[j])
        j += 1
    li[low:high+1] = ltmp

def _merge_sort(li, low, high):
    # 至少两个元素
    if low < high:
        mid = (low + high) // 2
        _merge_sort(li, low, mid)
        _merge_sort(li, mid+1, high)
        merge(li, low, mid, high)

def merge_sort(li):
    return _merge_sort(li, 0, len(li)-1)

li = list(range(1,9))
random.shuffle(li)
merge_sort(li)
```

## 希尔排序

希尔排序是一种分组插入排序算法

工作流程 : 

1. 首先取一个整数`d1=n/2` , 将元素分为d1个组 , 每组相邻两元素距离为d1 , 在各组内进行直接插入排序
2. 取第二个整数`d2=d1/2` , 重复上述分组排序过程 , 直到`d1=1` , 即所有元素在同一组内进行直接插入排序

希尔排序每躺并不使某些元素有序 , 而是使整体数据越来越进阶有序 ; 最后一趟排序使得所有数据有序

代码实现 : 

```python
import random
def shell_sort(li):
    gap = int(len(li) // 2)
    while gap > 0:
        for i in range(gap, len(li)):
            tmp = li[i]
            print(i-gap)
            j = i - gap
            while j >= 0 and tmp < li[j]:
                li[j + gap] = li[j]
                j -= gap
                li[j + gap] = tmp
                gap = int(gap / 2)

li = list(range(10000))
random.shuffle(li)
shell_sort(li)
print(li)
```

## 计数排序

现有一个列表 , 列表中的数范围都在0到100之间 , 列表长度大约为100万 , 设计算法在O(n)时间复杂度内将列表进行排序

工作流程 : 

1. 查找列表中最大和最小的元素
2. 开辟一个新的空间存放统计的每个元素出现次数
3. 反向填充目标列表

代码实现 : 

```python
import random
import copy
from timewrap import *

@cal_time
def count_sort(li, max_num = 100):
    count = [0 for i in range(max_num+1)]
    for num in li:
        count[num]+=1
    li.clear()
    for i, val in enumerate(count):
        for _ in range(val):
            li.append(i)

@cal_time
def sys_sort(li):
    li.sort()

li = [random.randint(0,100) for i in range(100000)]
li1 = copy.deepcopy(li)
count_sort(li)
'''
执行结果:
count_sort running time: 0.024517059326171875 secs.
'''
```

## 桶排序

桶排序的基本思想是将一个数据表分割成许多桶 , 然后每个桶各自排序 , 有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序

工作流程 :

1. 建立一堆buckets
2. 遍历原始数组 , 并将数据放入到各自的buckets当中
3. 对非空的buckets进行排序 
4. 按照顺序遍历这些buckets并放回到原始数组中即可构成排序后的数组

代码实现 : 

```python
import random
from timewrap import *

def list_to_buckets(li, iteration):
    """
    :param li: 列表
    :param iteration: 装桶是第几次迭代
    :return:
    """
    buckets = [[] for _ in range(10)]
    for num in li:
        digit = (num // (10 ** iteration)) % 10
        buckets[digit].append(num)
    return buckets

def buckets_to_list(buckets):
    return [num for bucket in buckets for num in bucket]

@cal_time
def radix_sort(li):
    maxval = max(li) 
    it = 0
    while 10 ** it <= maxval:
        li = buckets_to_list(list_to_buckets(li, it))
        it += 1
    return li

li = [random.randint(0,1000) for _ in range(100000)]
radix_sort(li)
'''
执行结果:
radix_sort running time: 0.3001980781555176 secs.
'''
```

## 小结

| 排序方法 | 时间复杂度(平均) | 时间复杂度(最坏) | 时间复杂度(最好) | 空间复杂度                         | 稳定性 | 复杂性 |
| -------- | ---------------- | ---------------- | ---------------- | ---------------------------------- | ------ | ------ |
| 冒泡排序 | O(n²)            | O(n²)            | O(n)             | O(1)                               | 稳定   | 简单   |
| 选择排序 | O(n²)            | O(n²)            | O(n²)            | O(1)                               | 不稳定 | 简单   |
| 插入排序 | O(n²)            | O(n²)            | O(n²)            | O(1)                               | 稳定   | 简单   |
| 快速排序 | O(nlogn)         | O(n²)            | O(nlogn)         | 平均情况O(nlogn)<br />最坏情况O(n) | 不稳定 | 较复杂 |
| 堆排序   | O(nlogn)         | O(nlogn)         | O(nlogn)         | O(1)                               | 不稳定 | 复杂   |
| 归并排序 | O(nlogn)         | O(nlogn)         | O(nlogn)         | O(n)                               | 稳定   | 较复杂 |
| 希尔排序 | O(nlogn)         | O(n²)            | O(n)             | O(1)                               | 不稳定 | 较复杂 |
| 计数排序 | O(n+k)           | O(n+k)           | O(n+k)           | O(n+k)                             | 稳定   | 简单   |
| 桶排序   | O(n+k)           | O(n²)            | O(n+k)           | O(n*k)                             | 不稳定 | 简单   |



