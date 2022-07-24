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
