# 冒泡排序

**冒泡排序** 比较所有相邻的两个项，如果第一个比第二个大，则交换它们。元素项向上移动至正确的顺序，就好像气泡升至表面一样，冒泡排序因此得名。

## 1. 思路

对于 n 项的数组，进行 n 轮冒泡循环，每一次冒泡循环中，从数组的第一项开始，和邻接索引递增的下一项进行比较，如果满足比较条件则交换双方索引，直至比较至最后一项。

## 2. 算法步骤

对于长度为 `n` 的数组 `a`，有比较条件 `compareFunc`，数组当前索引指针用 `i` 来表示，冒泡索引指针用`k`来表示：

1. 冒泡索引指针 `k` 指向 `0`。
2. 当前索引指针 `i` 指向 `k`。
3. 对于第 `i` 项如果满足 `compareFunc(a[i],a[i+1]) === Compare.BIGGER_THAN`，则交换数据项的索引索引。
4. 指针 `i` 指向相邻的下一个索引（`i+1`）。
5. 循环 `3.` `4.` 步骤,直到当前指针 `i` 处于数组第 `n` 项的索引上（`i === n-1`）。
6. 冒泡索引指针 `k` 指向相邻的下一个索引（`k+1`）
7. 循环 `2.` `3.` `4.` `5.` `6.` 步骤直到冒泡索引指针 `k` 处于数组的第 `n-1`项的索引上 (`k === n-1`)

## 3. 图示

![example](http://www.ituring.com.cn/figures/2019/JavaScriptLDSA/16.d13z.001.png)

## 4. 代码实现

### 简单实现

```javascript
function bubbleSort(array, compareFn = defaultCompare) {
  const { length } = array;
  for (let k = 0; k < length; k++) {
    for (let i = 0; i < length - 1; i++) {
      if (compareFn(array[i], array[i + 1]) === Compare.BIGGER_THAN) {
        swap(array, i, i + 1);
      }
    }
  }
  return array;
}
```

### 优化实现

```javascript
function optimizedBubbleSort(originArray, compareFn = defaultCompare) {
  // 保证源数据不可变
  const array = [...originArray];
  const { length } = array;
  let swaped = false;

  // 用 k 表示本轮冒泡后已排序的数组项数量，所以需要排序的数量是 length - k
  for (let k = 1; k < length; k++) {
    // 每轮冒泡开始都重新标记是否发生 swap
    let swaped = false;

    for (let i = 0; j < length - k; i++) {
      if (compareFn(array[j], array[i + 1]) === Compare.BIGGER_THAN) {
        swap(array, i, i + 1);

        // 标记已发生 swap
        swaped = true;
      }
    }
    // 如果该轮冒泡完成一次swap都没有发生，说明已完成排序，不需要继续执行循环。
    if (!swaped) {
      return array;
    }
  }
  return array;
}
```

## 复杂度

| 平均时间复杂度 | 最好情况 | 最坏情况 | 空间复杂度 | 稳定性 |
| -------------- | -------- | -------- | ---------- | ------ |
| O(n²)          | O(n)     | O(n²)    | O(1)       | 稳定   |
