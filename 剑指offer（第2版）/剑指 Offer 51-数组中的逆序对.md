## 剑指 Offer 51. 数组中的逆序对

[原题链接戳这⬅️](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

**示例 1:**

> **输入:** [7,5,6,4] 
> **输出:** 5

### 解题思路

 - 利用归并排序的思想
 - 合并左右两个有数子数组时，每当右侧子数组的数字并入，则**逆序对数量 += 左侧数组剩余的数字个数**，此时并入的数字小于左侧数组剩余的所有数字，均可构成逆序对

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        tmp = [0] * len(nums)
        
        def merge_sort(nums,l,r):
            if r == l + 1: return 0
            # 递归，分别对左右数组进行排序，逆序对数量相加
            mid = (l + r) // 2
            res = merge_sort(nums,l,mid) + merge_sort(nums,mid,r)
            i,j,k = l,mid,l
            tmp[l:r] = nums[l:r]
            while i < mid and j < r:
                if tmp[i] <= tmp[j]: 
                    nums[k] = tmp[i]
                    i += 1
                else:
                    nums[k] = tmp[j]
                    j += 1
                    # 逆序对数量 += 左侧数组剩余的数字个数
                    res += mid - i
                k += 1
            if i < mid: nums[k:k + mid - i] = tmp[i:mid]
            return res     

        return merge_sort(nums,0,len(nums))
```
