## 二分查找

**核心思想:** 将一段数据分为两部分,动态调整数据的边界,缩小查找数据的数据量

**整个数组是有序的**，数组默认是递增的。

- 首先选择数组中间的数字和需要查找的目标值比较
- 如果相等最好，就可以直接返回答案了
- 如果不相等
  - 如果中间的数字**大于**目标值，则**中间数字向右**的**所有数字都大于目标值**，全部排除
  - 如果中间的数字**小于**目标值，则**中间数字向左**的**所有数字都小于目标值**，全部排除

**两种写法:**

1. 左闭右闭,`right = array.length -1`,当middle大于target的时候更新right的值为 `right = middle - 1`
2. 左闭右开,`right = array.length`, 当middle大于target的时候更新right的值为 `right = middle`

![](D:\OfficeOnte\Mark文档\mdWord\assets\20210424160141149.png)

```javascript
//左闭右开的写法
var target = 436354
  var left = 0
  var right = array.length
  console.time('二分查找')
  function two() {
    while (left < right) {
      let middle = Math.floor(left + (right - left) / 2) //得到的结果可能是小数所以要向下取整
      if (array[middle] > target) {
        right = middle
      } else if (array[middle] < target) {
        left = middle + 1
      } else {
        return array[middle]
      }
    }
    return -1
  }
  console.log(two())
  console.timeEnd('二分查找')
```

```javascript
//效率比较
436354
传统: 3.887ms
[ 436354 ]
filter: 1.303ms
436354
二分查找: 0.299ms
```

