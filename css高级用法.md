### css3 calc()函数计算

```css
width: calc(100% - 80px); 
//乘法中必须有一个数时number
//除法 / 后面的数必须是number
//+ -运算符两边必须要有空格
//* /这两个运算符两边不能有空格
//用0做除数会抛出异常
```

### will-change 提高页面渲染性能

```css
will-change 为 web 开发者提供了一种告知浏览器该元素会有哪些变化的方法，这样浏览器可以在元素属性真正发生变化之前提前做好对应的优化准备工作
will-change: inherit;
will-change: initial;
will-change: revert;

will-change: margin, visibility, opacity
```

# clip-path 元素裁剪

属性使用裁剪方式创建元素的可显示区域。区域内的部分显示，区域外的隐藏
