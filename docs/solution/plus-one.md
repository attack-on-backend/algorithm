# 66.加一






<extoc></extoc>

## 题目

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

## 题解

### 思路一 - 反向遍历

反向遍历数组 , 遇 `9` 进 `1` , 遇到不等于 `9` 就结束遍历

注意 : 如果没有被 `break` 打断 , 需要在数组前补一位

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        for i in range(len(digits)):
            if digits[~i] == 9:
                digits[~i] = 0
            else:
                digits[~i] = digits[~i] + 1
                break
        else:
            digits.insert(0, 1)
        return digits
# 注: ~ 操作符为按位取反,就可以得到反向对称的索引
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(1)$

### 思路二 - 类型转换

数组转数字 , 加 `1` 操作后再打散成数组 , 不过这样做就脱离题意了

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        return [int(n) for n in str(int(''.join(list(map(lambda x: str(x), digits)))) + 1)]
```

**时间复杂度**：$O(N)$ , `N` 为数组长度

**空间复杂度**：$O(N)$ 



