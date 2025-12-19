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

#### clip-path 元素裁剪

属性使用裁剪方式创建元素的可显示区域。区域内的部分显示，区域外的隐藏

### content-visibility 渲染性能提升

content-visibility是CSS新增的属性，主要用来提高页面渲染性能，它可以控制一个元素是否渲染其内容，并且允许浏览器跳过这些元素的布局与渲染。

- `visible：`默认值，没有效果。元素的内容被正常布局和呈现。
- `hidden：`元素跳过它的内容。跳过的内容不能被用户代理功能访问，例如在页面中查找、标签顺序导航等，也不能被选择或聚焦。这类似于给内容设置display: none。
- `auto：`该元素打开布局包含、样式包含和绘制包含。如果该元素与用户不相关，它也会跳过其内容。与 hidden 不同，跳过的内容必须仍可正常用于用户代理功能，例如在页面中查找、tab 顺序导航等，并且必须正常可聚焦和可选择。

### **contain-intrinsic-size** 指定元素大小(和上面的属性搭配使用)

使用contain-intrinsic-size来指定的元素自然大小，确保我们未渲染子元素的 div 仍然占据空间，同时也保留延迟渲染的好处。

```css
.card_item {
  content-visibility: auto;
  contain-intrinsic-size: 200px;
}
```

### 根据字体大小自适应设置文字间距

```css
* {
   letter-spacing: clamp(
     -0.05em,
     calc((1em - 1rem) / -10),
     0em
   );
 }
```

