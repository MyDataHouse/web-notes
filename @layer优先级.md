### 1. 何为 CSS @layer？简单而言，[CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) [@规则](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule) 中的@layer声明了一个 级联层， 同一层内的规则将级联在一起， 这给予了开发者对层叠机制的更多控制。

```css
div {
    width: 200px;
    height: 200px;
}
@layer A {
    div {
        background: blue;
    }
}
@layer B {
    div {
        background: green;
    }
}
/*由于 @layer B 的顺序排在 @layer A 之后，所以 @layer B 内的所有
样式优先级都会比 @layer A 高，最终 div 的颜色为 green：*/
/*当然，如果页面内的 @layer 太多，可能不太好记住所有 @layer 的顺序
，因此，还有这样一种写法。我们可以同时命名多个 @layer 层，其后再补充其中的样式规则。*/
@layer B, C, A;
div {
    width: 200px;
    height: 200px;
}
@layer A {
    div {
        background: blue;
    }
}
@layer B {
    div {
        background: green;
    }
}
@layer C {
    div {
        background
: orange;
    }
}/*上述代码，我们首先定义了 @layer B, C, A 三个 @layer 级联层。而后再后面的 CSS 代码中补充了每个级联层的 CSS 代码，但是样式的优先级为：

A > C > B*/
```

### 2. @layer级联层的三种定义引入方式

1. 直接创建一个块级的 @layer 规则，其中包含作用于该层内部的CSS规则：
   
   ```css
   @layer utilities {
     p {
       padding: .5rem;
     }
   }
   ```
   
   2. 一个级联层可以通过 [@import](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import) 来创建，规则存在于被引入的样式表内:
      
      ```css
      @import(utilities.css) layer(utilities);
      ```
   
   3. 创建带命名的级联层，但不指定任何样式。样式随后可在 CSS 内任意位置添加：

```css
@layer utilities;
// ...
// ...
@layer utilities {
    p {
        color: red;
    }
}
```

### 3.  非 @layer 包裹层与 @layer 层内样式优先级

**非 @layer 包裹的样式，拥有比 @layer 包裹样式更高的优先级**，

### 4. 嵌套对@layer的优先级没有影响

### 5.  !important 对 CSS @layer 的影响

```css
div {
    width: 200px;
    height: 200px;
    background: black;
}
@layer A {
    div {
        background: blue;
    }
    @layer B {
        div {
            background: red;
        }
    }
}
@layer C {
    div {
        background: yellow;
    }
    @layer D {
        div {
            background: green!important;
        }
    }
}
```

如果，不考虑 `!important` 规则，那么实际的 CSS 优先级为（序号越高，优先级越高）：

1. @layer A.B
2. @layer A
3. @layer C.D
4. @layer C
5. 非 layer 包裹块

也就是说，这里 `!important` 规则的优先级还是凌驾于非 `!important` 规则之上的。

### 非 @layer 包含块 !important 与 @layer 包含块 !important

```css
div {
    width: 200px;
    height: 200px;
    background: black!important;
}
@layer A {
    div {
        background: blue;
    }
    @layer B {
        div {
            background: red;
        }
    }
}
@layer C {
    div {
        background: yellow;
    }
    @layer D {
        div {
            background: green!important;
        }
    }
}
```

这里最终 `<div>` 的颜色还是 `green`。这里就又有一个非常有意思的知识点了，!important @layer包含块下样式优先级的规则与非@layer包含块 !important 正常状态下刚好相反。
