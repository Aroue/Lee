# leetcode 刷题解析

## 一、数组

### 1、两数之和

```
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```



```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
      // 将每个元素的指和索引加入HashMap中
      Map<Integer, Integer> map = new HashMap<>();
      for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
      }
      // 根据nums[1] = target - nums[0]在HashMap中查找符合条件的两个数
      for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
          return new int[] { i, map.get(complement) };
        }
      }
      throw new IllegalArgumentException("No two sum solution");
    }
}
```

