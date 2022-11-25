### 列出一个数之前之后的数

```javascript
//计算2022年前后五年的年份
Array.from( Array(11), (item,index) => index + 2022 - 5 )
```

### 将数组对象转换为树形结构

```javascript
parseree(value, id){ //value是要转换的数组,id是入口标志,id是父级的pid
    let arr = []
   value.forEach(item => {
       if(item.pid===id){
           cosnt children = parseTree(item.id)
           if(children.length){
               item.children = children
           }
           arr.push(item)
       }
   })
    return arr
}
```

### 转换Excel日期格式

```javascript
// 转化excel的日期格式
export function formatDate(numb, format) {
  const time = new Date((numb - 1) * 24 * 3600000 + 1)
  time.setYear(time.getFullYear() - 70)
  const year = time.getFullYear() + ''
  const month = time.getMonth() + 1 + ''
  const date = time.getDate() - 1 + ''
  if (format && format.length === 1) {
    return year + format + month + format + date
  }
  return year + (month < 10 ? '0' + month : month) + (date < 10 ? '0' + date : date)
}
```



