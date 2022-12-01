# vue3和vue2中的不同点

### 1.`v-if`  `v-for`优先级顺序不同

- 在vue2中`v-for` 比`v-if`优先级高
- 在vue3中`v-if`比`v-for`优先级高

### 2.`v-for`中的`ref`数组

- 在vue2中，在`v-for`中使用`ref`来自动填充ref属性添加到`$refs数组`可以获取到

- 在vue3中，在`v-for`中使用`ref`来自动填充ref属性不会自动添加到`$refs数组`获取不到,可以自己写函数保存对应的元素对象上

	```javascript
	<template>
	    <div v-for='(item,index) in 5' :key=index :ref='setItemRef'>
	    </div>
	</template>
	<script>
	        data(){
	    return{
	        itemRefs:[]
	    }
	},
	methods:{
	    setItemRef(el){
	        if(el){
	            this.itemRefs.push(el)
	        }
	    }
	}
	</script>
	```

	

### 3.`$children`被废除

- vue2中访问当前实力的直接子组件
- vue3中已被**移除**，使用`$refs`访问子组件

### 4.`setup`语法糖-组合式API

组合式API来解决，使用（data,computed,methods,watch）等组件选项来组织逻辑通常很有效。然而，当我们的组件变得很大时，鼠标来回翻滚查看对应的逻辑代码，会很麻烦，这回让组件的代码阅读变得难以理解，所以使用组合式API可以解决这个问题

#### 4.1 在vue2和vue3中对data中的数据添加新属性

- vue2中对data中的对象或数组增加新属性后，新属性会失去响应式，要使用$set添加上

	```javascript
	data{
	    return{
	        a:{
	            a:1,
	            b:2
	        }
	    }
	}
	this.$set(this.a, 'c', 3)
	```

- 在vue3中只需要使用reactive包裹起来(需要的时候再加)

	```javascript
	import {ref, reactive} from 'vue' 
	//导入的是渲染函数
	setup(){
	    let obj = reactive(
	    	{
	            a:1,
	            b:2
	        }
	    )
	    let btn = ()=>{ obj.c=3 }
	    return {obj, btn}
	}
	```

- 渲染函数
	- ref：就是定义数据，什么类型的数据都可以定义， 一般时简单数据，数组时用它，拿数组数据时需要.value
	- reactive：也是定义数据，一般定义复杂类型，一般定义对象时用它
- 响应式区别
	- vue2: object.defineProperty()
	- vue3: Proxy()
		1. object.defineProperty()存在的问题
			1. 不能监听数组的变化
			2. 必须遍历对象的每一个属性，递归
		2. Proxy()
			1. 不需要遍历，只需要递归
			2. vue3已经做好了
