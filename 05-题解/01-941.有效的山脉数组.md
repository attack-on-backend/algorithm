# 941.有效的山脉数组






<extoc></extoc>

## 题目

给定一个整数数组 arr，如果它是有效的山脉数组就返回 true，否则返回 false。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：
```
arr.length >= 3
在 0 < i < arr.length - 1 条件下，存在 i 使得：
arr[0] < arr[1] < ... arr[i-1] < arr[i]
arr[i] > arr[i+1] > ... > arr[arr.length - 1]
```
<img style="height: 316px; width: 500px;" src="https://assets.leetcode.com/uploads/2019/10/20/hint_valid_mountain_array.png" alt="">

示例 1：
```
输入：arr = [2,1]
输出：false
```
示例 2：
```
输入：arr = [3,5,5]
输出：false
```
示例 3：
```
输入：arr = [0,3,2,1]
输出：true
```

提示：
```
1 <= arr.length <= 104
0 <= arr[i] <= 104
```
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-mountain-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 思路一 - 标志判断

定义一个标志位 , 标志是处于上升阶段还是下降阶段

注意 : 标志位最终结果必须标志着下降才是有效的山脉数组

```python
# flag = 0 标志着处于上升阶段
# flag = 1 标志着处于下降阶段
class Solution:
    def validMountainArray(self, arr: List[int]) -> bool:
        flag = 0
        for i in range(len(arr) - 1):
            if arr[i] == arr[i + 1]:
                return False
            # 上升
            if flag == 0:
                if arr[i] > arr[i + 1]:
                    if i == 0:
                        return False
                    flag = 1
            # 下降
            else:
                if arr[i] < arr[i + 1]:
                    return False
        if flag == 0:
            return False
        return True
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(1)$

### 思路二 - 双指针

利用双指针 , 分别同时从左右两边进行遍历 , 当左右指针都开始下降时停止 , 如果此时左右指针相等则为有效山脉 , 否则无效

注意 : 结束循环时 , 左指针不能是第一个元素 , 右指针不能是最后一个元素

```python
class Solution:
    def validMountainArray(self, A) -> bool:
        if len(A) < 3: return False
        left, right = 0, len(A) - 1
        while left < right:
            # 左指针右移
            if A[left] < A[left + 1]:
                left += 1
                continue
            elif A[left] == A[left + 1]:
                return False
            # 右指针左移
            if A[right] < A[right - 1]:
                right -= 1
                continue
            elif A[right] == A[right - 1]:
                return False
            # 如果左有指针都不移动了就结束循环
            break
        if left == right and right != len(A) - 1 and left != 0:
            return True
        return False
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(1)$