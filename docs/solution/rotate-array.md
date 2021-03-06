# 189.旋转数组






<extoc></extoc>

## 题目

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。


进阶：

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
你可以使用空间复杂度为 O(1) 的 原地 算法解决这个问题吗？


示例 1:
```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```
示例 2:
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

## 题解

### 思路一 - 切片拼接

以倒数第 `k` 位置为分界线 , 用切片切分出两个数组 , 再拼接两个数组 , 由于要求在原地操作所以最后再进行一下复制 (但是这样的话空间复杂度为 $O(N)$ 而不是 $O(1)$)

注意 : `k` 可能大于数组长度

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nums[:] = nums[len(nums) - k % len(nums):] + nums[:len(nums) - k % len(nums)]
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(N)$

### 思路二 - 环形换位

向右移动 `k` 个位置 , 或者是向左移动 `nums.length - k` 个位置 

向左换位 : 从尾部开始遍历 , 将第 `i` 个元素与第 `i - nums.length - k` 个元素互换 , 互换之前需要将被互换的位置的元素用变量存储起来再作为下一个换位的元素继续重复操作 , 整个换位过程刚好形成一个环

向右换位 : 从头部开始遍历 , 将第 `i` 个元素与第 `i + k` 个元素互换 , 互换之前同样需要将被互换的位置的元素用变量存储起来再作为下一个换位的元素继续重复操作 , 整个换位过程刚好形成一个环

注意 : 

- `k` 可能大于数组长度
- `k % len(nums) == 0 `  表示数组不移动
- 换位过程中可能有多个环 , 所以如果没有遍历完 , 当下一个索引回到了开始索引时 , 就应该往后挪一位继续换位操作

```python
# 以右移为例
class Solution:
    def rotate(self, nums, k) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if k % len(nums) == 0: return
        start, index, next_num = 0, 0, nums[0]
        for i in range(len(nums)):
            next_index = (index + k) % len(nums)
            next_num, nums[next_index] = nums[next_index], next_num
            index = next_index
            if next_index == start:
                index = start = start + 1
                next_num = nums[index]
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(1)$

### 思路三 - 三次反转

向右移 `k` 个位置 , 就是把从 `len(nums) - k`  到 `len(nums) -1 ` 的这段数组 , 与从 `0` 到 `len(nums) - k - 1` 这段数组调换一下位置 , 那我们可以 : 

1. 先反转整个数组 , 使两段数组的位置正确
2. 但是由于反转了整个数组 , 所以两段数组的位置顺序都被反转了 , 那么我们再分别反转这两段数组

```python
class Solution:
    def rotate(self, nums, k) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        self.reverse(0, len(nums) - 1, nums)
        self.reverse(0, k % len(nums) - 1, nums)
        self.reverse(k % len(nums), len(nums) - 1, nums)

    def reverse(self, start, end, nums):
        for i in range((end - start + 1) // 2):
            nums[start + i], nums[end - i] = nums[end - i], nums[start + i]
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(1)$

