#### 粘性定位：`position:sticky`  固定定位：`position:fixed`当元素祖先的 `transform`, `perspective` 或 `filter` 属性非 `none` 时，容器由视口改为该祖先。

#### Flex布局

##### `flex-direction`属性决定主轴的方向

1. `row`(默认值):主轴水平方向，起点在左边
2. `row-reverse`:主轴为水平方向，起点在右端
3. `column`:主轴为垂直方向，起点在上沿
4. `column-reverse`:主轴为垂直方向，起点在下沿

##### `flex-wrap`定义如果一条轴线排不下如何换行

1. `nowrap`:不换行
2. `wrap`: 换行，第一行在上方
3. `wrap-reverse`:换行，第一行在下方

##### `flex-flow`是`flex-direction`和`flex-wrap`的简写形式，默认为`row` `nowrap`

##### `justify-content`定义在***主轴***上对齐的方式

1. `flex-start`:（默认值）左对齐
2. `flex-end`:右对齐
3. `center`: 居中
4. `space-between`: 两端对齐，项目之间的间隔都相等
5. `space-around`:每个项目两侧的间隔相等，形目之间的间隔比项目与边框的大一倍

##### `align-items`定义在*交叉轴*上的对齐方式

1. `flex-start`:交叉轴的起点对其
2. `flex-end`: 交叉轴的终点对齐
3. `center`:交叉轴的中点对齐
4. `baseline`:项目的第一行文字的基线对齐
5. `stretch`:(默认值)如果项目未设置高度或设为auto,将占满整个高度,如果设置高度，这个属性没有效果

##### `align-content`属性定义了多跟轴线（多行项目）的对齐方式。如果只有一根轴线该属性不起作用

1. `flex-start`:与交叉轴的起点对齐
2. `flex-end`:与交叉轴的终点对齐
3. `center`:与交叉轴的中点对齐
4. `space-between`:与交叉轴两端对齐，轴线之间的间隔平均分布
5. `space-around`: 每个项目两侧的间隔相等，形目之间的间隔比项目与边框的大一倍
6. `stretch`:轴线占满整个交叉轴

##### 项目的属性

1. `order`: 定义项目的排列顺序。数值越小排列越靠前
2. `flex-grow`:定义项目的放大比例，默认为0
3. `flex-shrink`:定义项目的缩小比例，默认为1，及如果空间不足，该项目缩小
4. `flex-basis`:定义了再分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小，它的数值`带单位或百分比`，`优先级比width高变相定义了带这个属性项目的宽`。
5. `flex`:是上面几个属性的简写，默认值为0 1 auto。 后两个属性可选

##### `flex-basis`直接设置无效

```html
<view class='parent'>
    <view class='child1'></view>
    <view class='child2'></view>
    <view class='child3'></view>
</view>
```

```css
.parent { display: flex; }
.child1 { flex-basis: 100rpx; }
.child1 { flex-basis: 200rpx; }
.child3 { flex-grow: 1; }
```

这样不起作用（因为没有给父元素设置宽度）

```css
.child1 { flex: 0 0 100rpx; }
.child2 { flex: 0 0 200rpx; }
```

这样起作用（因为前两个值表示无论如何该元素都不会改变，会显示出来，`该元素没有内容的情况下，父元素要有高`。）

##### align-self`属性align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。