### 一.Ajax

#### 1.JQuery中数据的请求获取方法

```javascript
$.get('url',{},function(){});
$.post('url',{}function(){});
$.ajax({
    method:'get',
    url:'',
    data:{},
    seccess:function(res){};
})
```

#### 2.JQuery中快速获取表单数据

```javascript
let formData = $(formID).serialize();
```

### 二.原生XMLHttpRequest

#### 1.发起get请求

```javascript
var xhr = new XMLHttpRequest
xhr.open('GET','url');
xhr.send();
xhr.onreadystatechange=function(){
    if(xhr.readyState===4 && xhr.status === 200){
    console.log(xhr.responseText);
}
}
```

#### 2.发起post请求

```javascript
var xhr = new XMLHttpRequest;
xhr.open('POST','url');
xhr.setRrquestHeader('Content-Type','application/x-www-form-urlencoded')
xhr.send('bookname=水浒传&author=施耐庵&publisher=上海出版社');
xhr.onreadystatechange=function(){
    if(xhr.readystate === 4 && xhr.status === 200){
    console.log(xhr.reponseText);
}
}
```

### 三.URL编码转换

```javascript
//编码的方法
encodeURL()
//解码的方法
decodeURL()
//这个可以给对象生成一个字符串地址,在当前窗口关闭前有效
URL.createObjectURL()
```

### 四.Json

`JSON` 就是用字符串来表示 `Javascript` 的对象和数组。所以，`JSON` 中包含**对象**和**数组**两种结构，通过这

两种结构的相互嵌套，可以表示各种复杂的数据结构。

`JSON`语法注意事项

① 属性名必须使用双引号包裹

② 字符串类型的值必须使用双引号包裹

③ `JSON` 中不允许使用单引号表示字符串

④ `JSON` 中不能写注释

⑤ `JSON` 的最外层必须是对象或数组格式

⑥ 不能使用 `undefined` 或函数作为 `JSON` 的值

**`JSON` 的作用：**在计算机与网络之间存储和传输数据。

**`JSON` 的本质：**用字符串来表示 `Javascript` 对象数据或数组数据

#### 序列化和反序列化

把**数据对象** **转换为** **字符串**的过程，叫做**序列化**，例如：调用 `JSON.stringify()` 函数的操作，叫做 `JSON` 序列化。

把**字符串** **转换为** **数据对象**的过程，叫做**反序列化**，例如：调用 `JSON.parse()` 函数的操作，叫做 `JSON` 反序列化。

### 五.XMLHttpRequest Level2

- 可以设置 HTTP 请求的时限

- 可以使用 `FormData` 对象管理表单数据

- 可以上传文件

- 可以获得数据传输的进度信息

#### 设置`HTTP`请求时限

有时，`Ajax` 操作很耗时，而且无法预知要花多少时间。如果网速很慢，用户可能要等很久。新版本的 `XMLHttpRequest` 对象，增加了 `timeout` 属性，可以设置 `HTTP` 请求的时限：

```javascript
xhr.timeout = 300;
xhr.ontimeout = function(){
    alert('请求超时');
}
```

#### `FormData`对象管理表单数据

Ajax 操作往往用来提交表单数据。为了方便表单处理，`HTML5` 新增了一个 `FormData` 对象，可以模拟表单操作：

```javascript
// 1. 新建 FormData 对象
 var fd = new FormData()
 // 2. 为 FormData 添加表单项
 fd.append('uname', 'zs')
 fd.append('upwd', '123456')
 // 3. 创建 XHR 对象
 var xhr = new XMLHttpRequest()
 // 4. 指定请求类型与URL地址
 xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata')
 // 5. 直接提交 FormData 对象，这与提交网页表单的效果，完全一样
 xhr.send(fd)
```

#### FormData`对象管理表单数据

`FormData`对象也可以用来获取网页表单的值，示例代码如下：

```javascript
// 获取表单元素
var form = document.querySelector('#form1')
// 监听表单元素的 submit 事件
form.addEventListener('submit', function(e) {
 e.preventDefault()
 // 根据 form 表单创建 FormData 对象，会自动将表单数据填充到 FormData 对象中
 var fd = new FormData(form)
 var xhr = new XMLHttpRequest()
 xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata')
 xhr.send(fd)
 xhr.onreadystatechange = function() {}
})
```

#### 上传文件

```javascript
(() => {
                document.querySelector("#button2").addEventListener("click", (event) => {
                    event.preventDefault();
                    let file = document.querySelector("#file").files;
                    // files可以获得获取的文件列表
                    if (file.length <= 0) return alert("请选择上传文件");
                    //验证是否选择了文件
                    let fd = new FormData();
                    fd.append("avatar", file[0]);
                    //向表单对象添加数据
                    let xhr = new XMLHttpRequest();
                    xhr.open("POST", "http://ajax-base-api-t.itheima.net/api/upload/avatar");
                    xhr.send(fd);
                    xhr.onreadystatechange = function () {
                        if (xhr.readyState == 4 && xhr.status == 200) {
                            let data = JSON.parse(xhr.response);
                            if (data.status == 200) {
                                document.querySelector("img").src =
                                    "http://ajax-base-api-t.itheima.net" + data.url;
                            } else {
                                alert(data.message);
                            }
                        }
                    };
                });
            })();
```

### 显示文件上传进度

#### 计算文件上传进度

新版本的 `XMLHttpRequest` 对象中，可以通过监听 `xhr.upload.onprogress` 事件，来获取到文件的上传进度。语法格式如下：

```javascript
// 创建 XHR 对象
var xhr = new XMLHttpRequest()
// 监听 xhr.upload 的 onprogress 事件
xhr.upload.onprogress = function(e) {
     // e.lengthComputable 是一个布尔值，表示当前上传的资源是否具有可计算的长度
     if (e.lengthComputable) {
         // e.loaded 已传输的字节
         // e.total 需传输的总字节
         var percentComplete = Math.ceil((e.loaded / e.total) * 100)
     }
 }
```

#### 导入需要的库

```html
<link rel="stylesheet" href="./lib/bootstrap.css" />
<script src="./lib/jquery.js"></script>
```

#### 基于`Bootstrap`渲染进度条

```html
 <!-- 进度条 -->
 <div class="progress" style="width: 500px; margin: 10px 0;">
     <div class="progress-bar progress-bar-info progress-barstriped active" id="percent" style="width: 0%">
     0%
     </div>
 </div>
```

#### 动态设置到进度条上

```javascript
xhr.upload.onprogress = function(e) {
     if (e.lengthComputable) {
         // 1. 计算出当前上传进度的百分比
         var percentComplete = Math.ceil((e.loaded / e.total) * 100)
         $('#percent')
         // 2. 设置进度条的宽度
         .attr('style', 'width:' + percentComplete + '%')
         // 3. 显示当前的上传进度百分比
         .html(percentComplete + '%')
     }
}
```

### 监听上传完成的事件

```javascript
xhr.upload.onload = function() {
     $('#percent')
     // 移除上传中的类样式
     .removeClass()
     // 添加上传完成的类样式
     .addClass('progress-bar progress-bar-success')
}
```

# `jQuery`高级用法- `jQuery`实现文件上传（⭐⭐⭐）

## 定义`UI`结构

```html
 <!-- 导入 jQuery -->
 <script src="./lib/jquery.js"></script>
 <!-- 文件选择框 -->
 <input type="file" id="file1" />
 <!-- 上传文件按钮 -->
 <button id="btnUpload">上传</button>
```

## 验证是否选择了文件

```javascript
$('#btnUpload').on('click', function() {
     // 1. 将 jQuery 对象转化为 DOM 对象，并获取选中的文件列表
     var files = $('#file1')[0].files
     // 2. 判断是否选择了文件
     if (files.length <= 0) {
         return alert('请选择图片后再上传！‘)
     }
})
```

## 向`FormData`中追加文件

```javascript
// 向 FormData 中追加文件
var fd = new FormData()
fd.append('avatar', files[0])
```

## 使用`jQuery`发起上传文件的请求

```javascript
$.ajax({
     method: 'POST',
     url: 'http://www.liulongbin.top:3006/api/upload/avatar',
     data: fd,
     // 不修改 Content-Type 属性，使用 FormData 默认的 Content-Type 值
     contentType: false,
     // 不对 FormData 中的数据进行 url 编码，而是将 FormData 数据原样发送到服务器
     processData: false,
     success: function(res) {
         console.log(res)
     }
})
```

## `jQuery`实现`loading`效果

### `ajaxStart(callback)`

`Ajax` 请求**开始**时，执行 `ajaxStart` 函数。可以在 `ajaxStart` 的 `callback` 中显示 `loading` 效果，示例代码如下：

```javascript
// 自 jQuery 版本 1.8 起，该方法只能被附加到文档
$(document).ajaxStart(function() {
    $('#loading').show()
})
```

**注意：** `$(document).ajaxStart()` 函数**会监听当前文档内所有的 Ajax 请求**。

### `ajaxStop(callback)`

`Ajax` 请求**结束**时，执行 `ajaxStop` 函数。可以在 `ajaxStop` 的 `callback` 中隐藏 `loading` 效果，示例代码如下：

```javascript
// 自 jQuery 版本 1.8 起，该方法只能被附加到文档
$(document).ajaxStop(function() {
    $('#loading').hide()
})
```

# `axios（⭐⭐⭐）`

## 什么是`axios`

`Axios` 是专注于**网络数据请求**的库。

相比于原生的 `XMLHttpRequest` 对象，`axios` **简单易用**。

相比于 `jQuery`，`axios` 更加**轻量化**，只专注于网络数据请求。

## `axios`发起GET请求

`axios` 发起 `get` 请求的语法：

```javascript
axios.get('url', { params: { /*参数*/ } }).then(callback)
```

**具体的请求示例如下：**

```javascript
// 请求的 URL 地址
var url = 'http://www.liulongbin.top:3006/api/get'
// 请求的参数对象
var paramsObj = { name: 'zs', age: 20 }
// 调用 axios.get() 发起 GET 请求
axios.get(url, { params: paramsObj }).then(function(res) {
     // res.data 是服务器返回的数据
     var result = res.data
     console.log(res)
})
```

## `axios`发起`POST`请求

`axios` 发起 `post` 请求的语法：

```javascript
axios.post('url', { /*参数*/ }).then(callback)
```

**具体的请求示例如下：**

```javascript
// 请求的 URL 地址
var url = 'http://www.liulongbin.top:3006/api/post'
// 要提交到服务器的数据
var dataObj = { location: '北京', address: '顺义' }
// 调用 axios.post() 发起 POST 请求
axios.post(url, dataObj).then(function(res) {
     // res.data 是服务器返回的数据
     var result = res.data
     console.log(result)
})
```

## 直接使用`axios`发起请求

`axios` 也提供了类似于 `jQuery` 中 `$.ajax()` 的函数，语法如下：

```javascript
axios({
 method: '请求类型',
 url: '请求的URL地址',
 data: { /* POST数据 */ },
 params: { /* GET参数 */ }
}).then(callback)
```

**发起get请求**

```javascript
document.querySelector('#btn3').addEventListener('click', function () {
      var url = 'http://www.liulongbin.top:3006/api/get'
      var paramsData = { name: '钢铁侠', age: 35 }
      axios({
        method: 'GET',
        url: url,
        params: paramsData
      }).then(function (res) {
        console.log(res.data)
      })
})
```

**发起post请求**

```javascript
document.querySelector('#btn4').addEventListener('click', function () {
  axios({
    method: 'POST',
    url: 'http://www.liulongbin.top:3006/api/post',
    data: {
      name: '娃哈哈',
      age: 18,
      gender: '女'
    }
  }).then(function (res) {
    console.log(res.data)
  })
})
```

### 同源跨域

#### 同源策略

同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这

是一个用于隔离潜在恶意文件的重要安全机制

通俗的理解：浏览器规定，A 网站的 JavaScript，不允许和非同源的网站 C 之间，进行资源的交互，例如：

① 无法读取非同源网页的 Cookie、LocalStorage 和 IndexedDB

② 无法接触非同源网页的 DOM

③ 无法向非同源地址发送 Ajax 请求

#### 跨域

同源**指的是两个 URL 的协议、域名、端口一致，反之，则是** **跨域**

出现跨域的根本原因：**浏览器的同源策略**不允许非同源的 URL 之间进行资源的交互

网页：`http://www.test.com/index.html`

接口：`http://www.api.com/userlist同源**指的是两个 URL 的协议、域名、端口一致，反之，则是**跨域**

出现跨域的根本原因：**浏览器的同源策略**不允许非同源的 URL 之间进行资源的交互

网页：`http://www.test.com/index.html`

接口：`http://www.api.com/userlist

## 什么是`JSONP`(⭐⭐⭐)

`JSONP` (`JSON with Padding`) 是 `JSON` 的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。

## `JSONP`的实现原理(⭐⭐⭐)

由于浏览器同源策略的限制，网页中无法通过 Ajax 请求非同源的接口数据。但是 `<script>` 标签不受浏览器同

源策略的影响，可以通过 `src` 属性，请求非同源的 `js` 脚本。

因此，`JSONP` 的实现原理，就是通过 `<script>` 标签的 `src` 属性，请求跨域的数据接口，并通过**函数调用**的形式，接收跨域接口响应回来的数据

## 自己实现一个简单的`JSONP`

定义一个`success`回调函数：

```html
 <script>
     function success(data) {
     console.log('获取到了data数据：')
     console.log(data)
     }
 </script>
```

通过 `<script>` 标签，请求接口数据：

```html
<script src="http://ajax.frontend.itheima.net:3006/api/jsonp?callback=success&name=zs&a
ge=20"></script>
```

## `JSONP`的缺点

由于 `JSONP` 是通过 `<script>` 标签的 `src` 属性，来实现跨域数据获取的，所以，`JSONP` 只支持 `GET` 数据请求，不支持 POST 请求。

**注意：** **`JSONP` 和 Ajax 之间没有任何关系**，不能把 `JSONP` 请求数据的方式叫做 Ajax，因为 `JSONP` 没有用到

`XMLHttpRequest` 这个对象

## `jQuery`中的`JSONP`

`jQuery` 提供的 `$.ajax()` 函数，除了可以发起真正的 `Ajax` 数据请求之外，还能够发起 `JSONP` 数据请求，例如：

```javascript
$.ajax({
     url: 'http://ajax.frontend.itheima.net:3006/api/jsonp?name=zs&age=20',
     // 如果要使用 $.ajax() 发起 JSONP 请求，必须指定 datatype 为 jsonp
     dataType: 'jsonp',
     success: function(res) {
     console.log(res)
     }
})
```

默认情况下，使用 `jQuery` 发起 `JSONP` 请求，会自动携带一个 c`allback=jQueryxxx` 的参数，`jQueryxxx` 是随机生成的一个回调函数名称

### 自定义参数及回调函数名称

在使用 `jQuery` 发起 `JSONP` 请求时，如果想要自定义 `JSONP` 的**参数**以及**回调函数名称**，可以通过如下两个参数来指定：

```javascript
$.ajax({
     url: 'http://ajax.frontend.itheima.net:3006/api/jsonp?name=zs&age=20',
     dataType: 'jsonp',
     // 发送到服务端的参数名称，默认值为 callback
     jsonp: 'callback',
     // 自定义的回调函数名称，默认值为 jQueryxxx 格式
     jsonpCallback: 'abc',
     success: function(res) {
     console.log(res)
     }
})
```

### `jQuery`中`JSONP`的实现过程

`jQuery` 中的 `JSONP`，也是通过 `<script>` 标签的 `src` 属性实现跨域数据访问的，只不过，`jQuery` 采用的是**动态创建和移除标签**的方式，来发起 `JSONP` 数据请求。

- 在发起 `JSONP` 请求的时候，动态向 `<header>` 中 append 一个 `<script>` 标签；

- 在 `JSONP` 请求成功以后，动态从 `<header>` 中移除刚才 `append` 进去的 `<script>` 标签；

### 总结防抖和节流的区别

- **防抖**：如果事件被频繁触发，防抖能保证只有最有一次触发生效！前面 N 多次的触发都会被忽略！

- **节流**：如果事件被频繁触发，节流能够减少事件触发的频率，因此，节流是有选择性地执行一部分事件！
