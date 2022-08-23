## 数组相关的算法题目

### 01-题目描述
> 真题描述
> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
  
  
> 示例: 给定 nums = [2, 7, 11, 15], target = 9 
> 因为 nums[0] + nums[1] = 2 + 7 = 9 所以返回 [0, 1]
```js
/**
 * @param(number[]) nums
 * @param(number[]) nums
 * @return(number[])
*/
const twoSum = (nums, target) => {
    const mapTemp =new Map();
   for(let i =0; i<nums.length; i++) {
       const active = nums[i];
       const min = target - active;
       mapTemp.set(active, i);
       if (mapTemp.get(min)!== undefined) {
           return [i, mapTemp.get(min)]
       }
   }
}
```

### 02-题目描述
> 真题描述：给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。
> 说明: 初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。  
  
> 示例: 输入:
> nums1 = [1,2,3,0,0,0], m = 3
> nums2 = [2,5,6], n = 3
> 输出: [1,2,2,3,5,6]

```js

```