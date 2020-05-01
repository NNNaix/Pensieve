# 选择排序

**选择排序** 算法是一种原址比较排序算法。

## 1. 思路

选择排序大致的思路是找到数据结构中的最小值并将其放置在第一位，接着找到第二小的值并将其放在第二位，以此类推。

## 2. 算法步骤

对于长度为 `n` 的数组 `a`，有比较条件 `compareFunc`，数组当前索引指针用 `i` 来表示，最小值索引指针用 `m` 来表示，已遍历索引指针用 `k` 来表示：

1. 遍历索引指针 `k` 指向 `0`。
2. 当前索引指针 `i` 和最小值索引指针 `m` 指向遍历索引 `k`。
3. 对于第 `i` 项如果满足 `compareFunc(a[m],a[i]) === Compare.BIGGER_THAN`，则将最小索引指针 `m` 指向当前索引 `i`。
4. 指针 `i` 指向相邻的下一个索引（`i+1`）。
5. 循环 `3.` `4.` 步骤,直到当前指针 `i` 处于数组第 `n` 项的索引上（`i === n-1`）。
6. 如果遍历索引 `k` 和最小值索引 `m` 指向的索引不同，则交换数据项的索引。
7. 遍历索引指针 `k` 指向相邻的下一个索引（`k+1`）
8. 循环 `2.` `3.` `4.` `5.` `6.` `7.` 步骤直到遍历索引指针 `k` 处于数组的第 `n-2` 项的索引上 (`k === n-2`)

## 3. 图示

![example](http://www.ituring.com.cn/figures/2019/JavaScriptLDSA/16.d13z.003.png)

## 4. 代码实现

### 简单实现

```javascript
function selectionSort(array, compareFn = defaultCompare) {
  const { length } = array;
  let indexMin;
  for (let k = 0; k < length - 1; k++) {
    indexMin = k;
    for (let i = k; i < length; i++) {
      if (compareFn(array[indexMin], array[i]) === Compare.BIGGER_THAN) {
        indexMin = i;
      }
    }
    if (k !== indexMin) {
      swap(array, k, indexMin);
    }
  }
  return array;
};
```

## 复杂度

| 平均时间复杂度 | 最好情况 | 最坏情况 | 空间复杂度 | 稳定性 |
| -------------- | -------- | -------- | ---------- | ------ |
| O(n²)          | O(n²)     | O(n²)    | O(1)       | 不稳定   |
