### 对象

1.创建对象的方式

```javascript
//1.字面量
var obj = {};
//2.用new创建
var obj = new object();//var obj = new object({name:'值');
obj.name = '值';
//3.构造函数创建对象
function 构造函数名 (形参1,形参2){
    this.属性名1 = 参数;
    this.属性名2 = 参数;
}
var obj = new 构造函数名(实参1,实参2);
//4.函数返回值创建对象
function 函数名(形参1,形参2){
    return {属性名:参数,属性名:参数}
}
var obj = 函数名(实参1,实参2);
```

2.通过for in 来遍历对象

```javascript
for(var k in object){
    k//属性名
    object[k]//属性值
}
```

3.获取对象值

```javascript
var obj = {name:'我的世界'};
//1.
obj.name;
//2.
obj['name'];
```

4.给对象赋值

```javascript
var obj = {};
obj.name = '我的世界';
```

5. 删除属性
   
   ```javascript
   var obj = {};
   obj.name = '我的世界';
   delete obj.name;
   ```

### 内置对象方法：

#### 1. Math对象

    `Math`对象不是构造函数`Math`对象的所有属性与方法都是静态的。引用圆周率的写法是 `Math.PI`，调用正余弦函数的写法是 `Math.sin(x)`，`x` 是要传入的参数。`Math` 的常量是使用 JavaScript 中的全精度浮点数来定义的。

| 属性、方法名                | 功能                                                 |
| --------------------- |:-------------------------------------------------- |
| Math.PI               | 圆周率                                                |
| Math.floor()          | 向下取整                                               |
| Math.ceil()           | 向上取整                                               |
| Math.round()          | 四舍五入版 就近取整   注意 -3.5   结果是  -3                     |
| Math.abs()            | 绝对值                                                |
| Math.max()/Math.min() | 求最大和最小值                                            |
| Math.random()         | 获取范围在[0,1)内的随机值                                    |
| toFixed(保留的位数)        | 保留小数点后面几位,四舍六入五考虑，五后非零就进一，五后为零看奇偶，五前为偶应舍去，五前为奇要进一。 |

#### 2.日期对象

    Date对象是一个构造函数，需要通过new实列化后使用

```javascript
var date = new Date();//获取的是1970年到现在的毫秒数
var date = new Date('2022-4-14 0:0:0');//获取1970年到指定时间的毫秒数
var year = date.getFullYear();//获取当年
```

| 方法名           | 说明              | 代码                 |
| ------------- | --------------- |:------------------ |
| getFullYear() | 获取当年            | dobj.getFullYear() |
| getMonth()    | 获取当月(0-11)      | dobj.getMonth()    |
| getDate()     | 获取当天日期          | dobj,getDate()     |
| getDay()      | 获取星期几(周日0到周六6)  | dobj.getDay()      |
| getHours()    | 获取当前小时          | dobj,getHours()    |
| getMinutes()  | 获取当前几分钟         | dobj.getMinutes()  |
| getSeconds()  | 获取当前秒数          | dobj,getSeconds()  |
| Date.now()    | 获取格林威治时间到当前的毫秒数 | var a = Date.now() |

#### 3.数组对象

1. 创建数组的方式

```javascript
//字面量创建
var arr = [];

//new Array()
var arr = new Array();
//创建非空数组时，给new Array()传参，之传入一个参数时，规定了这个数组的长度
//如果传入了多个参数，那就会添加元素
```

2. 数组的增删查改
   
   | 方法名                     | 说明                                                                                       | 返回值                |
   | ----------------------- | ---------------------------------------------------------------------------------------- | ------------------ |
   | push(参数1，参数2)           | 从末尾添加一个或多个元素，修改原数组                                                                       | 返回新数组的长度           |
   | pop()                   | 从末尾删除一个元素，数组长度减一，修改原数组                                                                   | 返回删除元素的值           |
   | unshift(参数1，参数2)        | 从开头添加一个或多个元素，修改原数组                                                                       | 返回新数组的长度           |
   | shift()                 | 从开头删除一个元素，数组长度减一，修改原数组                                                                   | 返回删除的元素            |
   | arr.**concat**(数组1，数组2) | 连接两个或多个数组，不影响原数组                                                                         | 返回一个新数组            |
   | slice()                 | 数组截取slice(start,[end]);start的值表示从数组中的哪个位置开始可以为负值，负值表示倒数，end值可选表示到数组的哪个位置停止截取不会取到end      | 返回被截取的新数组          |
   | splice()                | 数组删除splice(第几个开始，要删几个数，要添加的元素);要删几个数可以为负数表示倒数，如果负值超过了数组本身的长度则归0，要删除的数如果超出了数组本身的大小会把数组清空。 | 返回被删除或新增的数组，会改变原数组 |
   | arr.toString()          | 把数组转化为字符串，逗号分割每一项。不影响原数组                                                                 | 返回一个字符串，           |
   | join('分隔符');            | 以分隔符为准分割每一项，转化为数组，不影响原数组                                                                 | 返回一个字符串            |
   | reverse()               | 颠倒数组中元素的顺序，改变原来的数组                                                                       | 返回新数组              |
   | sort()                  | 对数组的元素进行排序，改变原数组                                                                         | 返回新数组              |
   
   ES6新增方法
   
   | 方法名                          | 说明                             | 返回值                    |
   | ---------------------------- | ------------------------------ | ---------------------- |
   | Array.from(objarr,function)  | 把伪数组转换为真数组                     | 返回新数组                  |
   | find(item => item == b)      | 查找满足条件的第一个值                    | 返回符合条件的第一个值            |
   | findIndex(item => item == b) | 查找满足条件的第一个值的下标                 | 返回符合条件的第一个值的下标         |
   | includes()                   | 查找满足条件的值                       | 满足返回true,不满足返回false    |
   | reduce()                     | 重复执行函数并返回每次计算的值,通常用于计算数组,对象的总值 | 每次返回计算后的值,再次计算,        |
   | every()                      | 查找数组中不符合条件的直接返回false           | 不符合条件返回false符合条件返回true |
   
   3. 遍历数组的方法
   
   ```javascript
   var fruits = ["apple", "orange", "cherry"];
   fruits.forEach(myFunction);
   
   function myFunction(item, index,arr) {
     document.getElementById("demo").innerHTML += index + ":" + item + "<br>"; 
   }
   ```
   
   item代表当前数组的值，index为下标，arr数组对象

#### 4.字符串对象

1. <mark>字符串的不可变</mark>，指的是虽然看上去改变了内容但其实时地址变了，内存中开辟了新的空间

| 方法名                        | 说明                                                                         | 返回值               |
| -------------------------- | -------------------------------------------------------------------------- | ----------------- |
| indexOf('值'，fromIndex)     | indexOf() 方法返回调用它的 String 对象中第一次出现的指定值的索引，从 fromIndex 处进行搜索。如果未找到该值，则返回 -1 | 如果找到值返回下标,找不到返回-1 |
| lastIndexOf('值'，fromIndex) | 跟indexOf相反从后往前找，找到该值最后出现的位置                                                | 找到返回下标，找不到返回-1    |
| charAt(索引号)                | 根据索引号返回该位置的值，用来遍历字符串                                                       | 返回指定位置的值          |
| charCodeAt()               | 表示给定索引处的 UTF-16 代码单元,UTF-16 编码单元匹配能用一个 UTF-16 编码单元表示的 Unicode 码点           |                   |
| codePointAt()              | 方法返回 一个 Unicode 编码点值的非负整数。                                                 |                   |
| str.concat(str2,str3)      | 连接两个或多个字符串拼接字符等效于+ +                                                       | 返回新的字符串           |
| substr(start,length)       | 从start位置开始，length取的个数                                                      | 返回新的字符串           |
| slice(start,end)           | 从start开始截取到end位置,不包括end                                                    | 返回新的字符串           |
| substring(start,end)       | 从start开始截取到end位置,不包括end,不接受负值                                              | 返回新的字符串           |
| replace('被替换的值','替换的值')    | 利用循环和indexOf替换想要替换的值                                                       | 返回新的字符串           |
| split(’分割字符‘)              | 把字符串以分割字符为准转化为数组                                                           | 返回数组              |

#### ES6新增方法

| 方法名          | 说明            | 返回值        |
| ------------ | ------------- | ---------- |
| startsWith() | 字符串开头有没有指定的字符 | true,false |
| endsWith()   | 字符串结尾有没有指定的字符 | true,false |
| repeat()     | 将字符串重复的次数     | 返回新字符串     |

#### 5.检测数据类型

1. typeof检测数据类型
   
   ```javascript
   typeof arr == 'object';
   ```

2. instanceof检测数据类型
   
   ```javascript
   arr instanceof Array;//true
   arr instanceof String//flase
   ```

### API易忘点

#### 1.常见鼠标事件

| 鼠标事件          | 触发条件        |
| ------------- | ----------- |
| onclick       | 鼠标点击左键触发    |
| ondblclick    | 鼠标双击触发      |
| onmouseover   | 鼠标经过触发      |
| onmouseout    | 鼠标离开触发      |
| onmouseenter  | 鼠标经过触发，不会冒泡 |
| onmouseleave  | 鼠标离开触发，不会冒泡 |
| onfocus       | 获得鼠标焦点触发    |
| onblur        | 失去鼠标焦点触发    |
| onmousemove   | 鼠标移动触发      |
| onmouseup     | 鼠标弹起触发      |
| onmousedown   | 鼠标按下触发      |
| oncontextmenu | 控制右键菜单      |
| onselectstart | 控制选中文字      |

#### 2.操作class属性

1. 通过.style更改元素行内样式

2. 通过.className完全覆盖原有的class名称

3. 通过.classList方法添加删除替换类名
   
   ```javascript
   ul.classList.add('class');//添加类名
   ul.classList.remove('class');//删除类名
   ul.classList.toggle('reomve','class')//替换类名，有就添加，没有就删除低版本不支持
   ul.classList.contains('class')//判断class是否存在，返回布尔值
   ul.classList.length//返回类名的数量
   ul.classList.item(索引值)//根据索引值返回对应的类名
   ```

#### 3.自定义属性操作

1. 创建自定义属性
   
   ```javascript
   ul.setAttribute('data-index',1)//前面为属性，后面为属性值
   ```

2. 获得属性（与i可以获得内置属性）
   
   ```javascript
   ul.getAttribute('data-index');
   ul.dataset.index||ul.dataset['index']//H5新增
   ```

3. 移除属性
   
   ```javascript
   ul.reomveAttribute('data-index');
   ```

#### 4.节点操作

1. 父级节点
   
   ```javascript
   <ul><li></li></ul>
   li.parentNode//返回离Li最近的父节点，如果指定的节点没有父节点返回null
   ```

2. 子节点
   
   ```javascript
   ul.childNods//返回了ul包含的所有子节点，包括文本，元素节点，不使用
   ul.children//返回了ul包含的所有元素子节点
   ul.children[0]//返回第一个元素节点
   ul.children[ul.children.length-1]//返回最后一个元素节点
   ```

3. 子节点不经常使用的方法
   
   | 方法                   | 说明               |
   | -------------------- | ---------------- |
   | ul.firstChild        | 返回第一个子节点，包含文字节点  |
   | ul.lastChild         | 返回最后一个子节点，包含文字节点 |
   | ul.firstElementChild | 返回第一个元素节点，H5新增   |
   | ul.lastElementChild  | 返回最后一个元素节点，H5新增  |

4. 兄弟节点
   
   ```javascript
   ul.nextSibLing;//下一个兄第节点，包含元素，文字节点
   ul.previousSibLing//上一个兄弟节点，包含元素文字节点
   //H5新增兼容性问题
   ul.nextElementSibLing//下一个兄弟元素节点
   ul.previousElementSibling //上一个兄弟元素节点
   ```

5. nextSibLing问题解决
   
   ```javascript
   function getNextElementSibling(element) {
         var el = element;
         while (el = el.nextSibling) {
           if (el.nodeType === 1) {
               return el;
           }
         }
         return null;
       }  
   ```

##### 5.节点的增删

1. 创建节点（使用innerHTML创建多个元素时为了提高效率，把元素放到数组中一次性创建，此时效率比createElement高）
   
   ```javascript
   var li = document.createElement('li');//创建了一个Li节点
   ```

2. 添加节点
   
   ```javascript
   ul.appendChild(li);//给ul末尾添加了节点
   ul.insertBefore(li,ul.children[0])//第一个参数是要添加的节点
   //第二个是从什么位置开始
   ```

3. 删除节点
   
   ```javascript
   ul.removeChild(ul.children[0])//只能删除子节点
   ```

4. 克隆节点
   
   ```javascript
   ul.cloneNode(true);//参数为true表示深拷贝，不写或false表示浅拷贝
   ```

#### 6.事件操作

1. 事件注册的方式
   
   ```javascript
   //传统的注册方式，缺点只能注册一次
   ul.onclick = function(){}
   //h5新增，可以注册多次
   ul.addEventListener('click',function(){})
   //IE特有6 7 8支持
   ul.attachEvent('onclick',function(){})
   ```

2. 事件删除方式
   
   ```javascript
   //传统方式
   ul.onclick = function(){
       ul.onclick = null;
   };
   //H5新增方式
   function fn (){
       ul.removeEventListener('click',fn);
   }
   ul.addEventListener('click',fn);
   //IE特有 6 7 8 支持
   function fn1 (){
       ul.detachEvent('onclick',fn1);
   }
   ul.attachEvent('onclick',fn1);
   ```

#### 7.事件对象

1.  事件对象的使用
   
   ```javascript
   ul.onclick = function (event){
   var event = event || window.event;//兼容性写法为了兼容IE6-8
       console.log(e.target);
   
   }
   
   ul.onclick = function (e){
   var e = e || window.event
       console.log(e.target);
   
   }
   ```

2. 事件对象的属性和方法
   
   | 事件对象属性和方法           | 说明                          |
   | ------------------- | --------------------------- |
   | e.currentTarget     | 返回事件绑定的对象                   |
   | e.target            | 返回触发事件的对象                   |
   | e.srcElement        | 返回触发事件的对象IE6-8使用            |
   | e.type              | 返回事件的类型如：click              |
   | e.cancelBubble      | 该属性阻止冒泡，IE6-8使用             |
   | e.returnValue       | 该属性阻止默认事件，IE-6-8使用，比如不让链接跳转 |
   | e.preventDefault()  | 该方法组织默认事件，比如不让链接跳转          |
   | e.stopPropagation() | 阻止冒泡                        |

3. e.target和this的区别
   
   ```javascript
   <ul>
       <li>abc</li>
   </ul>
   <script>
       var document.querSelector('ul');
   ul.onclick = function(e){
   //点击li后,this指向绑定事件的ul，e.target指向触发事件的li
       console.log(this);//ul
       console.log(e.target);//li
   }
   </script>
   ```

4. 常见鼠标事件对象
   
   | 鼠标事件对象    | 说明                      |
   | --------- | ----------------------- |
   | e.clientX | 返回鼠标相对于浏览器窗口可视区的x坐标     |
   | e.clientY | 返回鼠标相对于浏览器窗口可视区的Y坐标     |
   | e.pageX   | 返回鼠标相对于文档页面的X坐标  IE9+支持 |
   | e.pageY   | 返回鼠标相对于文档页面的Y坐标  IE9+支持 |
   | e.screenX | 返回鼠标相对于电脑屏幕的X坐标         |
   | e.screenY | 返回鼠标相对于电脑屏幕的Y坐标         |

##### 7.2 键盘事件

| 键盘事件       | 触发条件                           |
| ---------- | ------------------------------ |
| onkeyup    | 某个键盘按键松开时触发                    |
| onkeydown  | 某个键盘按键按下时触发                    |
| onkeypress | 某个按键按下时触发，不识别功能键ctrl,shift,箭头等 |

##### 7.3 键盘事件对象

| 键盘事件对象属性         | 说明          |
| ---------------- | ----------- |
| ~~e.keyCode~~    | 返回改键的ASCII值 |
| e.key == 'enter' | 判断按下的按键     |

##### 7.4 window对象的常见事件

1. 页面加载事件
   
   ```javascript
   window.onload = function (){};
   window.addEventListener('load',function(){});
   ```

2. 调整窗口大小事件
   
   (只要发生窗口大小变化就会触发这个时间配合<mark>window.innerWidth</mark>使用)
   
   ```javascript
   window.onresize = function(){};
   window.addEventListener('resize',function(){});
   ```

##### 7.5 定时器

1. 延时定时器
   
   ```javascript
   //设置定时器
   var time = setTimeout(function,延迟的毫秒数)；
   //清除定时器
   clearTimeout(time);
   ```

2. 重复定时器
   
   ```javascript
   //设置定时器
   var time = setInterval(function, 毫秒数);
   //清楚定时器
   clearInterval(time);
   ```

##### 7.6 location对象

| location对象属性    | 返回值                 |
| --------------- | ------------------- |
| location.href   | 获取或设置整个url          |
| location.host   | 返回主机（域名）名           |
| location.port   | 返回端口号               |
| location.search | 返回参数                |
| location.hash   | 返回片段 #后面的内容 常见于链接锚点 |

###### 方法

| location对象方法       | 返回值                                         |
| ------------------ | ------------------------------------------- |
| location.assign()  | 跟href一样，可以跳转页面（网页重定向）                       |
| location.replace() | 替换当前页面，不记录历史，不能后退页面                         |
| location.reload()  | 重新加载页面，相当于刷新按钮或者 f5如果参数为true，强制刷新   ctrl+f5 |

##### 7.7 navigator对象

    navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 <mark>userAgent</mark>，该属性可以返回由客户机发送服务器的 user-agent 头部的值。

下面前端代码可以判断用户那个终端打开页面，实现跳转

```javascript
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
    window.location.href = "";     //手机
 } else {
    window.location.href = "";     //电脑
 }
```

##### 7.8 history对象

    window对象给我们提供了一个 history对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中）访问过的URL。

| history对象方法 | 作用                          |
| ----------- | --------------------------- |
| back()      | 可以后退功能                      |
| forward()   | 前进功能                        |
| go(参数)      | 参数如果为 1 是前进一个页面，是 -1 后退一个页面 |

#### 8.元素偏移量 offset

| offset属性     | 作用                                  |
| ------------ | ----------------------------------- |
| offsetparent | 返回作为该元素带有定位的父级元素，如果父级元素都没有定位则返回body |
| offsetTop    | 返回相对于带有定位父元素上方的偏移量                  |
| offsetLeft   | 返回相对于带有定位父元素左边框的偏移量                 |
| offsetWidth  | 返回自身包括padding,边框，内容区的宽度，            |
| offsetHeight | 返回自身包括padding,边框，内容区的高度，            |

#### 9.元素可视区 client

| client属性     | 作用                        |
| ------------ | ------------------------- |
| clientTop    | 返回元素上边框的大小                |
| clientLeft   | 返回元素左边框的大小                |
| clientWidth  | 返回自身包括padding,内容区的宽度，不含边框 |
| clientHeight | 返回自身包括padding,内容区的高度，不含边框 |

#### 10.元素滚动 scroll

| scroll属性           | 作用                          |
| ------------------ | --------------------------- |
| scrollTop          | 返回被卷曲的上侧距离                  |
| scrollLeft         | 返回被卷曲的左侧距离                  |
| scrollWidth        | 返回自身实际宽度，不含边框               |
| scrollHeight       | 返回自身实际高度，不含边框               |
| window.pageYOffset | 返回页面的被卷曲的上侧距离H5新增           |
| window.pageXOffset | 返回页面被卷去的左侧距离H5新增            |
| scroll(坐标x,坐标y)    | 将元素滚动到指定位置                  |
| scrollTo(坐标x,坐标Y)  | 将元素滚动到指定位置跟上面一样             |
| scrollBy(坐标x，坐标Y)  | 每隔一秒使元素从当前的位置向指定位置移动指定的像素位置 |

##### 兼容写法

```javascript
function getScroll() {
    return {
      left: window.pageXOffset || document.documentElement.scrollLeft || document.body.scrollLeft||0,
      top: window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0
    };
 } 
//document.documentElement.scroolTop要在页面声名dtd才能使用
```

页面被卷去的头部：可以通过<mark>window.pageYOffset</mark> 获得  如果是被卷去的左侧<mark>window.pageXOffset</mark>

#### 11.主要用法

1.offset系列 经常用于获得元素位置    offsetLeft  offsetTop

2.client经常用于获取元素大小  clientWidth clientHeight

3.scroll 经常用于获取滚动距离 scrollTop  scrollLeft  

4.注意页面滚动的距离通过 window.pageXOffset  获得

#### 12.移动端触摸事件

| 触屏事件       | 事件说明         |
| ---------- | ------------ |
| touchstart | 手指触摸到一个元素时触发 |
| touchmove  | 手指在触摸元素移动时触发 |
| touchend   | 手指离开元素时触发    |

##### 触摸事件对象

| 事件对象           | 事件说明                    |
| -------------- | ----------------------- |
| touches        | 正在触摸屏幕的所有手指的一个列表        |
| targetTouches  | 正在触摸当前DOM元素的手指列表        |
| changedTouches | 手指状态发生改变的手指列表，从无到有或从有到无 |

###### 1.动画过渡结束触发事件<mark>transitionend</mark>
