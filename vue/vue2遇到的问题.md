# vue 在项目实际项目的应用

### 1. vue在sass中使用data中的数据

#### 1.1在vue2中使用

```javascript
<template>
    <div :style='cssVars'></div>
</template>
```



```javascript
<script>
	computed:{
        cssVar(){
            return {
                "--color":this.color
            }
        }
    }
</script>
```

````javascript
<style>
    body{
        color:var(--color)
    }
</style>
````

#### 1.2 在vue3中使用

```javascript
<script>
    export default {
	data(){
    return {
        color:'#000'
    }
}
}
</script>
<style cssVars='{color}'>
    body{
        color:var(--color)
    }
</style>
```

#### 1.3  在使用变量时background属性中url不可以直接使用var()

```javascript
//正确使用方法,要在上述方法完成后
<script>
    export default {
	data(){
        return {
            bgimg:`url(\'${require('@/assets/images/1.jpg')}\')`
        }
    }
}
</script>
<style>
    body{
        background-image:var(--bgimg)
    }
</style>
```

### 2. 统一注册自定义组件

```javascript
//在components文件夹下创建index.js
import Nav from './Nav'
export default {
    install(Vue){
        Vue.component('Nav',Nav)
    }
}
//在main.js中引入
import components from '@/components'
Vue.use(components)
```

### 3. 统一挂载自定义指令

```javascript
//在src文件夹下创建directives文件夹创建index.js
export const imgerr(){
    //inserted也是钩子函数，节点插入父节点时调用
    inserted(dom,options){
        dom.onerror=()=>{
            dom.src = dom.src.trim() || options
            dom.src= options.value
        }
    }
}

//在main.js中引入
import * as directives from '@/directives'
Object.keys(directives).forEach(item=>{
    Vue.directive('item',directives[item])
})
```

### 4. 在vue全局挂载scss变量

- 添加vue.config.js配置

```javascript
module.exports={
    //全局样式↓=====================================================
  // npm install sass sass-loader -D
  // 如果 Node Sass 与脚手架版本不兼容，先卸载
  css: {
    loaderOptions: {
      scss: {
          //变量文件所在地址
        additionalData: `@import "@/assets/style/globalVariable.scss";`
        //旧版本用prependData，新版比如cli5本用additionalData
        //注意：sass-loader将文件引用写入每个组件，适合全局引入变量，但不适合在单页面应用中添加样式，如果是全局样式（非变量），建议在main.js里引入
      }
    }
  },
  //全局样式↑=====================================================
}
```

