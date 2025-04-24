# Attack on Algorithm - 双指针 🐝 

## 定义

**双指针 : **指的是在遍历元素的过程中 , 不是使用单个指针进行访问 , 而是使用两个指针进行访问 , 从而达到相应的目的 , 如果两个指针方向相反 , 则称为对撞指针 , 如果两个指针方向相同 , 则称为快慢指针 , 如果两个指针分别属于不同的数组 / 链表 , 则称为分离指针

双指针通常分为三类 : 对撞指针、快慢指针、滑块指针

## 对撞指针

**对撞指针**：指的是两个指针 $left$、$right$ 分别指向序列第一个元素和最后一个元素 , 然后 $left$ 指针不断递增 , $right$ 不断递减 , 直到两个指针的值相撞 , 即 $left==right$ , 或者满足其他要求的特殊条件为止

![two-pointer-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/two-pointer-01.png?raw=true)

代码模板

```python
left, right = 0, len(nums) - 1

while left < right:
    if 满足要求的特殊条件:
        return 符合条件的值 
    elif 一定条件 1:
        left += 1
    elif 一定条件 2:
        right -= 1

return 没找到 或 找到对应值
```

对撞指针一般用来解决有序数组或者字符串问题 : 

- 查找有序数组中满足某些约束条件的一组元素问题 : 比如二分查找、数字之和等问题
- 字符串反转问题 : 反转字符串、回文数、颠倒二进制等问题

## 快慢指针

**快慢指针 : **指的是两个指针从同一侧开始遍历序列 , 且移动的步长一个快一个慢 , 两个指针以不同速度、不同策略移动 , 直到快指针移动到数组尾端 , 或者两指针相交 , 或者满足其他特殊条件时为止

![two-pointer-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/two-pointer-02.png?raw=true)

代码模板

```python
slow = 0
fast = 1
while 没有遍历完：
    if 满足要求的特殊条件:
        slow += 1
    fast += 2
return 合适的值
```

快慢指针一般用于处理数组中的移动、删除元素问题 , 或者链表中的判断是否有环、长度问题

## 滑块指针

**滑块指针 :** 指的是将两个指针的区间范围当作一个滑块 , 然后将这个窗口在数组上滑动 , 在窗口滑动的过程中 , 左边会出一个元素 , 右边会进一个元素 , 然后计算对比当前窗口内的元素即可

![two-pointer-03](https://github.com/attack-on-backend/algorithm/blob/master/assert/two-pointer-03.png?raw=true)

代码模板

```python
left = 0
right = k
while 没有遍历完
  自定义逻辑
  left += 1
  right += 1
return 合适的值
```

滑块指针一般用来解决一些查找满足一定条件的连续区间或长度问题

## 例题

对撞指针

- [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)
- [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

快慢指针

- [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
- [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

滑块指针

- [219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

- [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)
