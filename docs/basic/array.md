# Attack on Algorithm - 数组 🐝 










<extoc></extoc>

## 讲义

[leetcode - array-and-string](https://leetcode-cn.com/leetbook/detail/array-and-string/)

## 例题

### 66.加一

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。


示例 1：
```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```
示例 2：
```
输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
```
示例 3：
```
输入：digits = [0]
输出：[1]
```

提示：
```
1 <= digits.length <= 100
0 <= digits[i] <= 9
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/plus-one
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

### 941.有效的山脉数组

给定一个整数数组 arr，如果它是有效的山脉数组就返回 true，否则返回 false。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：
```
arr.length >= 3
在 0 < i < arr.length - 1 条件下，存在 i 使得：
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


### 189.旋转数组

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。


进阶：

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
你可以使用空间复杂度为 O(1) 的 原地 算法解决这个问题吗？
 

示例 1:
```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```
示例 2:
```
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

提示：
```
1 <= nums.length <= 2 * 104
-231 <= nums[i] <= 231 - 1
0 <= k <= 105
```
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。