# from表单

### 去掉`input textarea`的默认样式

1. `iniput:text/search`去掉获取焦点时出现的边框
  
   1. `input{outline:none}`
   2. 在标签内设置`maxlength`规定文本框内的最大数值
   3. 在标签内设置`placeholder`设置文本框内的默认显示文本

2. `textarea`控制文本域缩放
  
   1. `textare{resize:none}`（禁止缩放）

3. `required`设置表单必填项

## `radio`单选框实现多选一，必须加`name`值并且名字要一样

`checkbox`的`name`值也要相同。

1. 在标签内添加checkd值默认选中该选项

## `lable`标签的用法

```html
<lable for='sex'>男</lable>
<input type='radio' name='sex' id='sex' />
```

## `select`表单元素的用法

```html
<select>
  <option selectd='selectd'>选项1</option>
  <option>选项2</option>
  <option>选项3</option>
</select>
```

### 禁止表单自动填充文本框

```javascript
autocomplete = 'off'
```

### 禁用按钮

```javascript
<button type='submit' disabled='true'>
```

### 设置表单元素为只读

```javascript
<input type='text' readonly>
```



## 获取文件的方法,value

```javascript
//方法一通过e对象,获取File类型对象
function(e){
    let file = e.target.files[0];
}
//放法2,获取File类型对象
function(){
    let file = $('[input=file]')[0].files[0];
}
```

1. 表单获取的文件路径在value属性中,想要重复选择,要清空value,触发change事件 

2. 使用window.URL.createObjectURL()来吧对象转换成URL路径

### 上传文件时修改文件名

```javascript
//假设这就是选择文件后执行的函数
//由于文件对象的属性是只读的所以只有重新创建文件对象修改名字
function(e){
    let file = e.target.files[0];
 file = new File([file],`123_${file.name}`,{type:file.type, lastModified:file.lastModified})
}
```

### 上传文件修改文件编码格式FileReader

```javascript
//假设这就是选择文件后执行的函数
//把图片转换欸base64格式
//上传base64图片使用application/x-www-form-urlencoded 的请求头以字符串为文件主体上传时要处理编码问题
//转换的base64字符串进行编码，服务器解码  encodeURIComponent()
//encodeURIComponent() 相比 encodeURL的转义范围更大
function(e){
    let file = e.target.files[0];
    let fileReader = new FileReader()
    fileReader.readAsDateURL(file)
    fileReader.onload = ev=>{
        console.log(ev.target.result)
    }
}
```

