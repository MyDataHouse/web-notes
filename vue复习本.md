# vue复习本

### vue语法修饰符号

<mark>再进行判断对象或数组长度的时候要在前面加上该对象或数组进行短路这样就不会报错</mark>

arr&&arr.length

| vue完整写法                                             | 精简写法 | 说明                  |
| --------------------------------------------------- | ---- | ------------------- |
| v-bind:                                             | :    | 添加表签固定属性值           |
| v-on:                                               | @    | 注册事件                |
| v-model                                             |      | 获取表单的值              |
| v-text                                              |      | innerText  相同       |
| v-html                                              |      | innerHTML  相同       |
| v-show                                              |      | 隐藏方式display:none    |
| v-if                                                |      | 隐藏方式直接移除标签,可以进行条件判断 |
| v-for="(item,index) in arr" ;key =index             |      | 循环数组或对象,根据数据创建标签    |
| :class="{类名 : bool}"或:class=[active === 1?"类名":" "] |      | 动态添加属性名             |
| :style="{css属性名 : 值}"                               |      | 动态修改样式              |
| v-slot:'slotname'                                   | :    | 具名插槽                |
|                                                     |      |                     |
|                                                     |      |                     |
|                                                     |      |                     |
|                                                     |      |                     |
|                                                     |      |                     |
|                                                     |      |                     |
|                                                     |      |                     |
|                                                     |      |                     |

#### v-model高级用法

一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，但是像单选框、复选框等类型的输入控件可能会将 `value` attribute 用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。`model` 选项可以用来避免这样的冲突：

```javascript
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

现在在这个组件上使用 `v-model` 的时候：

```javascript
<base-checkbox v-model="lovingVue"></base-checkbox>
```



### vue修饰符

| 符号     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| .stop    | 停止冒泡                                                     |
| .prevent | 阻止默认事件                                                 |
| .once    | 只执行一次的事件                                             |
| .number  | 把数字字符串,转换成数字                                      |
| .trim    | 去除两边多余的空格                                           |
| .lazy    | 失去焦点提交值                                               |
| .native  | 给组件原始标签添加原生事件,鼠标事件大多不用加,键盘事件要添加这个修饰符 |
| .sync    | 子组件向父组件传值的语法糖,子组件通过$emit('updata:父组件传的值的名称',值) |
|          |                                                              |
|          |                                                              |
|          |                                                              |
|          |                                                              |
|          |                                                              |
|          |                                                              |

### vue生成阶段

| 方法名           | 阶段                    |     |
| ------------- | --------------------- | --- |
| beforeCreate  | 初始化前,挂载data,methods前, |     |
| created       | 初始化后,挂载data,methods后  |     |
| beforeMount   | 挂载前,用来挂载组件            |     |
| mounted       | 挂载后                   |     |
| beforeUpdate  | 更新前,dom更新前            |     |
| updated       | 更新后,dom更新后            |     |
| beforeDestroy | 销毁前,组件销毁前             |     |
| destroyed     | 销毁后,组件销毁后             |     |

### vue自带方法

| 方法名    | 说明                                    |
| --------- | --------------------------------------- |
| $on       | 绑定自定义事件                          |
| $emit     | 触发自定义事件                          |
| $nextTick | dom更新后依次触发                       |
| $refs     | 配合red获取dom元素                      |
| $nextTick | dom更新后执行的                         |
| $event    | 获取事件对象,自定义事件获取的是传的参数 |
| $parent   | 获取父组件的实例化对象,调用父组件的方法 |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |
|           |                                         |

### vue自定义指令

```javascript
//在单独的文件中定义自定义指令再在main.js中通过循环注册指令
export const imagerror = { //指令名,使用时加上v-
  inserted: function(dom, options) {  //dom渲染就执行,使用指令的节点,options是传入指令的参数
    dom.src = dom.src.trim() || options
    dom.onerror = function() {
      dom.src = options.value
    }
  },
  componentUpdated(dom, options) { //dom更新后就执行
    dom.src = dom.src || options.value
  }
}

//main.js中注册指令
import directives form '@/directives'//导入自定义指令文件
Object.keys(directives).forEach(item => {
  // 遍历注册自定义命令
  Vue.directive(item, directives[item])
})
```



### vue组件传值

1. 父组件向子组件传值
  
   ```javascript
   //父组件
   <template>
   <div>
       <son :arr='arr'></son>
   </div>
   </template>
   
   //子组件
   <template>
   <div>
       {{arr.name}}
   </div>
   </template>
   <script>
   export defalut {
       props:['arr']
   //或者
       props:{
       arr:{
       type:Array,
       //复杂数据类型默认值用函数返回,简单数据类型直接写就可以了
       default:()=>[]
       validator(val){
       if(val.length<=0)return false
   }
   }
   }
   }
   </script>
   ```

2. 子组件向父组件传值
  
   ```javascript
   //父组件
   <template>
   <div>
       <son @add='add'></son>
   </div>
   </template>
   <script>
   export default{
       methods:{
       add(val){
       this.name = val
   }
   }
   }
   </script>
   
   //子组件
   <template>
   <div>
   <button @click='add'>点击传值</button>
   </div>
   </template>
   <script>
   export default{
       methods:{
       add(val){
       this.$emit('add',val)
   }
   }
   }
   </script>
   ```

3. 没有关系的组件之间传值
  
   ```javascript
   //main.js中全局添加eventBus对象
   
   Vue.prototype.$eventBus = new Vue()
   ```
   
   ```javascript
   <template>
   <div>
       显示值{{name}}
   </div>
   </template>
   <script>
   export default{
       data(){
       return {
       name:''
   }
   }
   created(){
       this.$eventBus.$on('addName',val=>{
       this.name = val
   })
   }
   }
   </script>
   ```
   
   ```javascript
   <template>
   <div>
       传值
   </div>
   </template>
   <script>
   export default{
   created(){
       this.$eventBus.$emit('addName','穿的名字')
   }
   }
   </script>
   ```

4. 但很多子组件需要用到父组件的某一个值可以通过依赖注入的方式向所有子组件传值
  
   ```javascript
   //字符组件中添加如下代码
   provide: function () {
     return {
       getMap: this.getMap
     }
   }
   //在子组件间中添加如下代码,校验方法跟props相同
   inject: ['getMap']
   ```

### 动态组件

```javascript
<template>
  <div>
      <button @click="comName = 'UserName'">账号密码填写</button>
      <button @click="comName = 'UserInfo'">个人信息填写</button>

      <p>下面显示注册组件-动态切换:</p>
      <div style="border: 1px solid red;">
          <component :is="comName"></component>
      </div>
  </div>
</template>

<script>
// 目标: 动态组件 - 切换组件显示
// 场景: 同一个挂载点要切换 不同组件 显示
// 1. 创建要被切换的组件 - 标签+样式
// 2. 引入到要展示的vue文件内, 注册
// 3. 变量-承载要显示的组件名
// 4. 设置挂载点<component :is="变量"></component>
// 5. 点击按钮-切换comName的值为要显示的组件名

import UserName from '../components/01/UserName'
import UserInfo from '../components/01/UserInfo'
export default {
    data(){
        return {
            comName: "UserName"
        }
    },
    components: {
        UserName,
        UserInfo
    }
}
</script>
```

1. 由于这样切换会每次都销毁组件重复切换会影响性能可以在组件外包含一个标签不让他销毁

```javascript
<keep-alive>//让组件不销毁
          <component :is="comName"></component>
</keep-alive>
```

2. 被缓存的组件不再创建和销毁, 而是激活和非激活
  
    <mark>activated – 激活时触发</mark>
   
    <mark>deactivated – 失去激活状态触发</mark>

### 组件插槽

```javascript
<templete>
<div>这是一个组件</div>
<slot>这是一个预留插槽</slot>
</templete>
```

可以在组件中插入不确定的内容,在组件中使用<mark>slot</mark>标签预留插槽位置即可,在插入的时候在组件标签内写上要插入的标签

#### 具名插槽

```javascript
//组件中
<div>
<slot name='title'></slot>    
</div>
```

```javascript
//App.vue中
<templete>
 <zujian>
  <templete v-slot:title>//固定语法
    <h1>插入的内容</h1>
  </templete>
 </zujian>
</templete>
//引用组件
.........
```

#### 作用域插槽

```javascript
//组件中
<div>
<slot :row='obj'>
    {{obj.name}}
</slot>    
</div>
exprot default:{
    data:{
    obj:{
    name:'张三',
    oname:'李四'
}
}
}
```

```javascript
//App.vue中
<templete>
 <zujian>
  <templete v-slot='scope'>//固定语法,scope名字随意,也可以直接结构{row}
    {{scope.row.oname}}
  </templete>
 </zujian>
</templete>
//引用组件
.........
```

在组件内传值给调用组件的主组件,主组件在使用值

# vue router

1. 下载包 vue-router

2. 在mian.js中入包
  
   ```javascript
   import VueRouter from 'vue-router'
   ```

3. 注册全局组件
  
   ```javascript
   Vue.use(VueRouter)
   ```

4. 定义路由规则
  
   ```javascript
   const routers = [{
       path:'/my',
       component:'my'
   },{
       .......
   }]
   ```

5. 实例化路由器
  
   ```javascript
   const router = new VueRouter({
   //添加路由规则
       routers
   
   })
   ```

6. 跟vue实例关联
  
   ```javascript
   new Vue({
       router,
       render:(h)=>h(App),
   }).$mount('#app');
   ```

7. 在App.vue中使用
  
   ```javascript
   <template>
       <router-link to='/my'>我的</router-link>//声明式路由
       <div>    
           <router-view></router-view>//路由的组件位置
       </div>
   </template>
   ```

# vuex

### 导入vuex

```java
import Vuex from 'vuex'
//配置vuex

const stor = new Vuex.stor({
    state:{
    name:'李四'
}
    mutations:{
    	jia(state,val){
    	state.name = val
}
}
})
new Vue({
    stor,
    render:(h)=>h(App);
}).$mount('#app')
```

### 使用state中的值

```javascript
this.$stor.state.name
//可以在computed:{}中设置
computed:{
    name(){
    return this.$stor.state.name
}
}
//官方方法
<script>
import {mapState} from 'vuex'
computed:{
    ...mapState{['name']}//可以直接使用name,原理同上
}
</script>
```

### 使用 mutations改变state中的数据

```javascript
this.$stor.$mutations.jia(this.$stor.state,'数据')

//在methods中设置
methods:{
    fn(val){
    this.$stor.commit('方法名',canshu)
}
}
//官方方法
<script>
import {mapMutations} from 'vuex'
methods:{
    ...mapMutations(['jia'])//直接调用就好
}
</script>
```

### 异步更改state的值actions

```javascript
//定义actions中的方法
actions:{
    fn(context,val){
        context.commit('mutiations中定义的方法',params)
    }
}
//在组件中通过actions改变state中的值
<script>
import  {mapActions} from 'vuex'
methods:{
    ...mapActions['fn']
}
//第二种方法
this.$store.dispatch('fn',参数)
</script>
```

### getters对数据进行加工,类似计算属性

1. 定义

   ```javascript
   state:{
       list:[1,2,3]
   }
   getters:{
       filterList: state => state.list.filter(item => item > 5)
   }
   ```

2. 使用

   ```javascript
   //第一种方法
   this.$store.getters.filterList
   //第二种方法
   import {mapGetters} from 'vuex'
   computed:{
       ...mapGerrers(['filterList'])
   }
   ```

   

## 模块化

```javascript
//导入子vuex模块
import list from './list'
modules:{
    list //绑定
}
```

```javascript
//子模块
export default {
    state:{}
    mutations:{}
}
```

在模块中除了state中的属性外都是共享的,可以通过之前的方法直接调用,但这样会造成模块之间的混乱,可以使用,<mark>namespaced:true</mark>来让各自的方法独立

### 独立命名空间的调用方法

1. 按照之前的方法调用,路径上加上子路径,但是不能使用map*方法

   ```javascript
   this.$store.commit('list/fn',val)
   ```

2. 使用createNameSpacedHelpers

   ```javascript
   import {createNameSpacedHelpers} from 'vuex'
   const {mapMutations:listMutations} = createNameSpacedHelpers('list')
   methods:{
       ...listMutations(['fn'])
   }
   ```

   

### 子模块之间的互相调用

```javascript
//在调用时传入第三个参数{ root : true }
context.commit('permission/setRoutes', [], { root: true })
```

