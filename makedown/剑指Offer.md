## 数组中重复的数字

- 题目
  - 返回一个重复的值
- 解法
  - 遍历
  - 哈希表
    - 通过contained方法去判断
  - 原地交换
    - 当1位置上的值不是1时，让1位置上的和值对应的下标进行交换，直到值相同就操作下一位
    - 当出现当前位置的值是之前的值时返回