### sass不经常使用会忘记的的嵌套规则

```scss
.funky {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
/*以下是在scss中编译出的样式*/
.funky {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold; }
```

#### 命名空间也可以包含自己的属性值

```scss
.funky {
  font: 20px/24px {
    family: fantasy;
    weight: bold;
  }
}
/*编译后*/
.funky {
  font: 20px/24px;
    font-family: fantasy;
    font-weight: bold; 
}
```

#### `%`占位符选择器不会编译到css中与`@extend`配合使用<mark>插值语句在注释中同样有效</mark>

#### `/**/`注释会被编译到css文件中`//`不会编译到css文件中

### scss中的变量与赋值

```scss
$width: red !global;
/*声明了一个width的变量并赋值red,同时也可以赋值字符串,
将局部变量升级为全局变量添加!global*/
```

### @if语句判断条件达成包含的内容

```scss
$type: monster;
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == matador {
    color: red;
  } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
/*输出为*/
p {
  color: green; 
}
```

### @for语句`@for` 指令可以在限制的范围内重复输出格式，每次按要求（变量的值）对输出结果做出变动。这个指令包含两种格式：`@for $var from <start> through <end>`，或者 `@for $var from <start> to <end>`，区别在于 `through` 与 `to` 的含义：*当使用 `through` 时，条件范围包含 `<start>` 与 `<end>` 的值，而使用 `to` 时条件范围只包含 `<start>` 的值不包含 `<end>` 的值*。另外，`$var` 可以是任何变量，比如 `$i`；`<start>` 和 `<end>` 必须是整数值。

```scss
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
/*编译为*/
.item-1 {
  width: 2em; }
.item-2 {
  width: 4em; }
.item-3 {
  width: 6em; }

/*另一个例子*/
@for $nobmer from 1 to 13 {
  .cl-sm-#{$nobmer} {
    width: unquote($nobmer / 12 * 100 + "%");
  }
}
/*编译为*/
.cl-sm-1 {
  width: 8.3333333333%;
}

.cl-sm-2 {
  width: 16.6666666667%;
}

.cl-sm-3 {
  width: 25%;
}

.cl-sm-4 {
  width: 33.3333333333%;
}

.cl-sm-5 {
  width: 41.6666666667%;
}

.cl-sm-6 {
  width: 50%;
}

.cl-sm-7 {
  width: 58.3333333333%;
}

.cl-sm-8 {
  width: 66.6666666667%;
}

.cl-sm-9 {
  width: 75%;
}

.cl-sm-10 {
  width: 83.3333333333%;
}

.cl-sm-11 {
  width: 91.6666666667%;
}

.cl-sm-12 {
  width: 100%;
}
```

### @each指令把后面的值每个都拿出来单独使用，读取字符串的内容

```scss
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
/*编译为in后面的值也可以是包含字符串的变量*/
.puma-icon {
  background-image: url('/images/puma.png'); }
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); }
.egret-icon {
  background-image: url('/images/egret.png'); }
.salamander-icon {
  background-image: url('/images/salamander.png'); }
/*跟index()函数配合使用读取下标*/
$string:red green blue;
@each $name in $string{
    $index:index($string,$name);
    .p#{$index - 1}{
    background-color: $name;
}
}
/*这里把$string里的颜色值每一个都取出来，
并赋值给.p而.p获取下标后每次加一成为.p1.p2.p3*/
```

### `@while` 指令重复输出格式直到表达式返回结果为 `false`。这样可以实现比 `@for` 更复杂的循环，只是很少会用到

```scss
$li: 0;
@while ($li<=12) {
  .cl-sm-#{$li} {
    width: unquote($li / 12 * 100 + "%");
  }
  $li: $li + 1;
}

/*编译后*/
```
