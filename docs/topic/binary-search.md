# Attack on Algorithm - 二分法 🐝 

## 定义

**二分查找算法 (Binary Search Algorithm) :** 也叫做折半查找算法、对数查找算法 , 是一种用于在有序数组中查找特定元素的高效搜索算法

二分查找的基本算法思想为 : 通过确定目标元素所在的区间范围 , 反复将查找范围减半 , 直到找到元素或找不到该元素为止

二分查找是一种特殊的分支 , 每次只处理一个子问题

## 实现

实现二分查找算法的具体步骤 : 

1. **初始化 :** 首先 , 确定要查找的有序数据集合 , 可以是一个数组或列表 , 确保其中的元素按照升序或者降序排列。
2. **确定查找范围 :** 将整个有序数组集合的查找范围确定为整个数组范围区间 , 即左边界 $left$ 和右边界 $right$
3. **计算中间元素 :** 根据 $mid=(left+right)/2$ 计算出中间元素下标位置 $mid$
4. **比较中间元素 :** 将目标元素 $target$ 与中间元素 $nums[mid]$ 进行比较 : 
   1. 如果 $target==nums[mid]$ , 说明找到 $target$ , 因此返回中间元素的下标位置 $mid$
   2. 如果 $target<nums[mid]$ , 说明目标元素在左半部分 $[left,mid−1]$ , 更新右边界为中间元素的前一个位置 , 即 $right=mid−1$
   3. 如果 $target>nums[mid]$ , 说明目标元素在右半部分$[mid+1,right]$ , 更新左边界为中间元素的后一个位置 , 即 $left=mid+1$
5. 重复步骤 3∼4 , 直到找到目标元素时返回中间元素下标位置 , 或者查找范围缩小为空 (左边界大于右边界) , 表示目标元素不存在 , 此时返回 −1

模板代码

```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

## 例题

- [704. 二分查找](https://leetcode.cn/problems/binary-search/)
- [374. 猜数字大小](https://leetcode.cn/problems/guess-number-higher-or-lower/)
- [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)



